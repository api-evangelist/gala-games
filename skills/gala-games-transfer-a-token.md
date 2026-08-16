---
name: Transfer a token on GalaChain
description: Move a fungible or non-fungible token between GalaChain wallets by signing a DTO and posting it to the gateway, with a dry run first and idempotent retries.
api: openapi/gala-games-galachain-asset-token-contract-openapi.json
base_url: https://gateway-mainnet.galachain.com/api/asset/token-contract
operations:
  - asset_token-contract_FetchBalances
  - asset_token-contract_DryRun
  - asset_token-contract_TransferToken
  - asset_public-key-contract_GetObjectByKey
generated: '2026-08-16'
method: generated
---

# Transfer a token on GalaChain

GalaChain is REST over chaincode. There is no bearer token: you sign the request body with a secp256k1 wallet key and the chain verifies it against the public key registered on chain. Every write carries a `uniqueKey` that makes retries safe.

## Before you start

You need three things: a wallet address (`client|<24 hex>` or `eth|<40 hex>`), the wallet's private key, and its public key registered on chain. Verify registration with `asset_public-key-contract_GetObjectByKey` against `https://gateway-mainnet.galachain.com/api/asset/public-key-contract/GetObjectByKey` before anything else. An unregistered key produces a signature failure that looks like a malformed request.

## 1. Read the balance first — no credential needed

Reads are unauthenticated. POST to `asset_token-contract_FetchBalances` with the owner and the token class key:

```json
{ "owner": "eth|abcd...", "collection": "GALA", "category": "Unit", "type": "none", "additionalKey": "none" }
```

Token identity is the composite tuple `collection|category|type|additionalKey`. `"none"` is a required literal, not a null. A specific NFT unit adds `instance`; fungible balances use instance `"0"`.

Confirm the balance covers both the transfer quantity **and** the fee. Fees are separate and denominated in GALA.

## 2. Quote the fee

Some operations cost GALA. Quote before you commit rather than discovering it on failure. On GalaConnect, append `/fee` to any route path and POST the same body with `signature` and `uniqueKey` omitted — for example `POST https://api-galaswap.gala.com/v1/channels/{channel}/AuthorizeFee/fee`.

Two fee types behave differently. `galachain_automatic` is deducted for you on commit and needs no action. `galachain_cross_channel_authorization` must be paid **in advance** via `POST /v1/channels/{channel}/AuthorizeFee`, which burns GALA on the asset channel and credits a fee allowance on the target channel. You hit the second type when the token lives on a game channel rather than `asset` — which is the case for most NFTs. You can batch it: authorize ten GALA once, then perform ten one-GALA transfers.

## 3. Build the DTO

```json
{
  "from": "eth|abcd",
  "to": "eth|dcba",
  "tokenInstance": {
    "collection": "GALA", "category": "Unit", "type": "none",
    "additionalKey": "none", "instance": "0"
  },
  "quantity": "1",
  "uniqueKey": "<globally unique string>",
  "dtoExpiresAt": 1755300000000
}
```

`quantity` and `instance` are BigNumber-as-string. Do not send them as JSON numbers — precision is lost silently.

Set `dtoExpiresAt` to a few minutes out (the docs use `Date.now() + 300000`). It bounds replay exposure independently of `uniqueKey`. If it is in the past, the DTO is invalid.

## 4. Choose the uniqueKey deliberately

`uniqueKey` is mandatory and it is the idempotency contract. GalaChain will not permit two transactions with the same `uniqueKey` to commit; the second is rejected with `UniqueTransactionConflictError`.

Generate one UUID **per logical transfer** and reuse it across every retry of that transfer. Do not generate a fresh key on retry — that is how you send the tokens twice. On GalaConnect the key must be prefixed `galaconnect-operation-`.

Unlike a conventional `Idempotency-Key` header, this key lives in the signed body, so a retry must resend the byte-identical signed payload. Sign once, store the signed payload, replay that.

## 5. Sign

Signing is exact and order-sensitive:

1. Remove any existing `signature` property.
2. Recursively sort every property alphabetically by name.
3. Stringify to a minimal deterministic JSON string.
4. keccak256 hash it.
5. Sign the hash with secp256k1.
6. Normalize `s` to at most half the curve order `n`.
7. DER encode, base64, and set as `signature`.

Use `@gala-chain/api`'s `createValidDTO` plus `.sign(privateKey)` rather than hand-rolling this — `createValidDTO` also catches the field errors that would otherwise surface as a 401. From a shell, `galachain dto:sign ./key dto.json` does the same thing.

Add `signerPublicKey` as base64. For MetaMask signatures also set `prefix`. For multisig, supply `multisig` (minimum 2 entries) and `signerAddress`, omit `signature` and `signerPublicKey`, and set `dtoOperation` to `channelId_chaincodeId_methodName`.

## 6. Dry run

POST the signed DTO to `asset_token-contract_DryRun` first. It evaluates without committing and returns the writes it would make. This is the cheapest way to catch an insufficient balance, a missing allowance or an unregistered key, and it costs nothing.

## 7. Submit

```
POST https://gateway-mainnet.galachain.com/api/asset/token-contract/TransferToken
Content-Type: application/json
```

For a token on a game channel, swap the channel segment: `/api/mirandus/token-contract/TransferToken`.

## 8. Read the response correctly

An HTTP 200 does not mean the transfer succeeded. The body is a `GalaChainResponse`:

```json
{ "Status": 1, "Data": { ... } }
```

`Status: 1` is success and `Data.txid` is the transaction id. On failure the same envelope carries `Message`, `ErrorCode`, `ErrorKey` and `ErrorPayload`. **Always branch on `Status` and `ErrorKey`, never on the HTTP status alone.** When a contract method throws, no state changes are saved, but the failed transaction is still recorded in transaction history.

## Handling errors

- `UniqueTransactionConflictError` — your `uniqueKey` already committed. This is the idempotency guarantee working. Treat it as confirmation of success and stop; do not retry with a new key.
- HTTP 409 without that key — a serialization failure from concurrent writes to the same wallet. Serialize writes per wallet and retry with the *same* signed payload.
- HTTP 401 on GalaConnect — signature validation failed. The usual causes are unsorted properties before stringification, an un-normalized `s` value, a `signerPublicKey` that is not base64, or a key not yet registered on chain.
- HTTP 429 on GalaConnect — the global limit is 20 requests per 10 seconds. Honour the `Retry-After` header, in whole seconds.

## Do not

- Do not parallelise writes for one wallet.
- Do not mint a new `uniqueKey` on retry.
- Do not trust HTTP 200 as success.
- Do not send quantities as JSON numbers.

## References

- Conventions: `conventions/gala-games-conventions.yml`
- Errors: `errors/gala-games-problem-types.yml`
- Authentication: `authentication/gala-games-authentication.yml`
- Gala's own worked example: https://docs.galachain.com/latest/integration-guide/
