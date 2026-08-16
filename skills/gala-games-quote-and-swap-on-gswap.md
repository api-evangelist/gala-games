---
name: Quote and execute a DEX swap on GalaSwap
description: Price a trade against GalaChain concentrated-liquidity pools, execute it through the DeFi backend or the dexv3 contract, and confirm settlement.
api: openapi/gala-games-defi-backend-openapi.json
base_url: https://dex-backend-prod1.defi.gala.com
operations:
  - TradeController_quote
  - TradeController_getPoolDetails
  - TradeController_getCompositePool
  - TradeController_getSlot0
  - TradeController_getPrice
  - TradeController_swap
  - TradeController_bundle
  - TradeController_transactionStatus
  - UserController_fetchAssets
  - UserController_fetchGalaBalance
  - asset_dexv3-contract_GetCompositePool
  - asset_dexv3-contract_GetSlot0
  - asset_dexv3-contract_DryRun
  - asset_dexv3-contract_BatchSubmit
generated: '2026-08-16'
method: generated
---

# Quote and execute a DEX swap on GalaSwap

GalaSwap is a Uniswap-v3-style concentrated-liquidity DEX on GalaChain. Two surfaces reach it and they are not interchangeable:

- **Gala DeFi Backend** (`https://dex-backend-prod1.defi.gala.com`) — 316 operations. Quoting, analytics, positions and a convenience layer. Public read endpoints need no credential; admin endpoints require `X-Api-Key`.
- **dexv3 contract via the gateway** (`https://gateway-mainnet.galachain.com/api/asset/dexv3-contract`) — 21 operations. The settlement layer. Signed DTOs only.

Quote on the backend. Settle on chain.

## 1. Check what you hold

`UserController_fetchAssets` — `GET /user/assets` — returns holdings with metadata. `UserController_fetchGalaBalance` — `GET /user/fetch-gala-balance` — returns GALA specifically, which matters because GALA pays the fees as well as being a tradeable asset. Leave headroom for both.

## 2. Find the pool

A pool is identified by `token0`, `token1` and `fee` (the fee tier). Ordering is not arbitrary — `token0` and `token1` are canonically ordered, so build the pair from the pool response rather than from your own intent, or you will read prices inverted.

- `TradeController_getPoolDetails` — `GET /v1/trade/pool`
- `TradeController_getCompositePool` — `GET /v1/trade/composite-pool` — the fuller picture including tick data
- `TradeController_getCompositePoolBatch` — `POST /v1/trade/composite-pool/batch` — many pairs in one call; use it instead of a loop, given there is no published rate limit to rely on
- `TradeController_getSlot0` — `GET /v1/trade/slot0` — current `sqrtPrice` and tick

For discovery rather than a known pair: `ExploreController_poolsDetails` (`GET /explore/pools`), `PairsController_getPairs` (`GET /dex/pairs`), and `ExploreController_opportunities` (`GET /explore/opportunities`).

## 3. Quote

`TradeController_quote` — `GET /v1/trade/quote`. Quote both directions of intent explicitly. The `SwapDTO` schema distinguishes them:

- **Exact input**: set `amountIn` and `amountOutMinimum`.
- **Exact output**: set `amountOut` and `amountInMaximum`.

`amountOutMinimum` / `amountInMaximum` are your slippage protection and they are the whole safety story for an automated trade. Compute them from the quote and your tolerance, and always send them. `sqrtPriceLimit` bounds how far the trade may move the pool — set it when you are large relative to liquidity.

Every amount is a BigNumber-as-string. Never a JSON number.

For a second opinion, price the same trade against the chain directly with `asset_dexv3-contract_GetCompositePool` and `asset_dexv3-contract_GetSlot0`. The backend can be cached; the contract is authoritative.

## 4. Dry run before signing

`asset_dexv3-contract_DryRun` evaluates a signed DTO without committing and returns the writes it would make. Run it. It costs nothing and it catches insufficient balance, missing swap allowance and unregistered keys before you spend a fee.

If the pool needs an allowance first, `asset_dexv3-contract_GrantSwapAllowance` (or `GrantBulkSwapAllowance`) grants it; `asset_dexv3-contract_FetchSwapAllowances` shows what is already granted.

## 5. Execute

Through the backend: `TradeController_swap` — `POST /v1/trade/swap`. Through the chain: `asset_dexv3-contract_BatchSubmit`. Either way the payload is a signed DTO carrying `uniqueKey`, `signature` and `signerPublicKey` — the signing procedure is identical to a token transfer, see `gala-games-transfer-a-token.md`.

`TradeController_bundle` (`POST /v1/trade/bundle`) and `TradeController_bundleMultiple` combine operations into one submission. Use them when a flow is genuinely atomic — a swap plus a liquidity add — rather than to batch unrelated work.

Note `TradeController_exec` (`POST /v1/trade/exec`) is **marked deprecated** in the spec, with no replacement named. Do not build on it.

## 6. Confirm settlement

`TradeController_transactionStatus` — `GET /v1/trade/transaction-status`. Poll it with the transaction id from the submit response.

Do not assume HTTP 200 means the trade executed. The backend wraps responses in `BaseResponseDto` (`status`, `message`, `error`, `data`); the gateway returns `GalaChainResponse` (`Status`, `Data`, `Message`, `ErrorCode`, `ErrorKey`, `ErrorPayload`). Branch on the envelope, not the transport.

## Liquidity, if you are providing it

- `TradeController_addLiquidityEsimate` / `TradeController_removeLiquidityEsimate` — model first
- `TradeController_addLiquidity` (`POST /v1/trade/liquidity`) / `TradeController_removeLiquidity` (`DELETE /v1/trade/liquidity`)
- `TradeController_getUserPositions` / `TradeController_getUserPosition`
- `TradeController_collect` — collect accrued fees
- `asset_dexv3-contract_TransferDexPosition` — move a position between wallets

Positions are bounded by `tickLower` and `tickUpper`. Some incentive programs require full-range positions — check `ActiveIncentiveProgramResponse.requiresFullRange` and `minPositionValueUsd` via `IncentiveController_getActivePrograms` before you choose a range, or you will build a position that does not qualify.

## Idempotency and concurrency

`uniqueKey` is mandatory on every write and is enforced at the ledger. Generate one per logical trade and reuse it across retries — a fresh key on retry executes the trade twice. `UniqueTransactionConflictError` means the trade already landed; stop.

Do not run concurrent writes for one wallet. Serialization failures come back as HTTP 409.

## Rate limits

The DeFi backend publishes none. The only documented Gala limit is GalaConnect's 20 requests per 10 seconds, and it does not apply here. No Gala surface emits `X-RateLimit-*` headers. Batch where a batch operation exists, and back off on any 429.

## References

- Conventions: `conventions/gala-games-conventions.yml`
- Data model: `data-model/gala-games-data-model.yml`
- Fees and monetization: `plans/gala-games-plans-pricing.yml`
- Lifecycle and deprecations: `lifecycle/gala-games-lifecycle.yml`
