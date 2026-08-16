---
name: Read GalaChain state without credentials
description: Query blocks, transactions, balances, allowances, bridge operations and channel inventory from the Gala Block Explorer API — the whole surface is open and needs no key.
api: openapi/gala-games-block-explorer-openapi.json
base_url: https://explorer-api.galachain.com
operations:
  - AppController_getHealth
  - CachedExplorerController_getRegisteredChannels
  - CachedExplorerController_getRecentBlocks
  - CachedExplorerController_getBlockByNumber
  - CachedExplorerController_getBlockHeightOnChannel
  - CachedExplorerController_getRecentTransactionsByChannel
  - CachedExplorerController_search
  - ExplorerController_getBalances
  - ExplorerController_getAllowances
  - ExplorerController_getBridgeOps
  - CachedExplorerController_getTokenMedia
generated: '2026-08-16'
method: generated
---

# Read GalaChain state without credentials

The Gala Block Explorer API is the most agent-friendly surface Gala operates: 17 operations, no authentication, no API key, no wallet. Start every read-only integration here rather than at the gateway.

Base URL: `https://explorer-api.galachain.com`. Interactive docs at `/docs/`, machine-readable spec at `/docs-json`.

## Check liveness first

`AppController_getHealth` — `GET /health`. Gala publishes no status page for any of its APIs, so this endpoint and the DeFi backend's `/api/v1/wallet/compat/health` are the only availability signals that exist. If you are building anything durable, poll one of them yourself; there is no dashboard to watch and no incident feed to subscribe to.

## Discover the channels

`CachedExplorerController_getRegisteredChannels` returns the live channel list. Do not hard-code it. Mainnet currently carries eighteen channel/contract pairs: `asset` (with `token-contract`, `dexv3-contract`, `launchpad-contract`, `fee-contract`, `public-key-contract`) plus a `token-contract` on each of `championsarena`, `echoesofempire`, `eternalparadox`, `galafilm`, `lastexpedition`, `legacy`, `legendsreborn`, `mirandus`, `music`, `node`, `superior`, `thewalkingdeadempires` and `vox`. Testnet carries two more.

Channel selection matters everywhere downstream: a Mirandus NFT is not on the `asset` channel, and operating on it incurs a cross-channel fee.

## Walk blocks and transactions

- `CachedExplorerController_getRecentBlocks` / `CachedExplorerController_getRecentBlocksByChannel` — the tail of the chain.
- `CachedExplorerController_getBlockByNumber` — a specific block.
- `CachedExplorerController_getBlockHeightOnChannel` — current height, the cheapest way to detect that a channel advanced.
- `CachedExplorerController_getTransactions` / `..._getTransactionsByChannel` / `..._getRecentTransactionsByChannel`.

The `Cached` prefix is a real hint about latency, not a naming accident — these operations serve a cache rather than the peer. Gala describes the data as "near real-time". Do not build a settlement check that assumes the explorer is instantly consistent with the gateway: confirm a write from the `GalaChainResponse.Data.txid` the gateway returned, then use the explorer for history and enrichment.

Paginated responses carry `paging` with `limit`, `offset`, `size` and `count`. This is offset pagination, unlike the gateway's bookmark pagination — the two surfaces do not share a convention.

## Look up balances and allowances

- `ExplorerController_getBalances` — holdings for an address.
- `ExplorerController_getAllowances` — delegated permissions granted to or by an address, including `quantitySpent`, `uses`, `usesSpent` and `expires`.

Token identity is the composite key `collection|category|type|additionalKey`, with `instance` for a specific unit. `"none"` is a required literal value. `GALA|Unit|none|none` is the native token.

## Track bridge operations

`ExplorerController_getBridgeOps` returns cross-chain bridge activity. Pair it with the gateway's `asset_token-contract_FetchTokenBridgeStatus` when you need the status of one specific bridge-out request rather than the feed.

## Search

`CachedExplorerController_search` takes a `SearchRequestDTO` (`search`, `limit`, `offset`, `deepSearch`) and resolves across blocks and transactions. It is the right entry point when you hold an opaque identifier and do not yet know what it is. `deepSearch` costs more — leave it off for interactive lookups.

## Fetch token art

`CachedExplorerController_getTokenMedia` takes a token class key and returns `TokenClassImageDTO` entries. This is the only published route to token imagery.

## What this API cannot do

It is read-only. Nothing here writes, and nothing here is authenticated, which is exactly why it is the right first stop for an agent. When you need to write, move to the gateway (`gala-games-transfer-a-token.md`) or GalaConnect (`gala-games-create-and-fill-a-swap.md`) and bring a wallet.

It also has **no MCP tools** — Gala's only MCP server covers the Launchpad product, so an agent reaching this API does so over plain HTTP.

## Rate limits

None are published for this API. The documented 20-per-10-seconds limit applies to GalaConnect only. Absence of a published limit is not a promise of no limit: back off on any 429 and honour `Retry-After` if it appears.

## Errors

Failures return `{"statusCode": ..., "message": ..., "error": ...}` — the NestJS default shape, not the `GalaChainResponse` envelope you get from the gateway and not the `errorId` shape you get from GalaConnect. Three surfaces, three envelopes; handle each where you meet it.

The Block Explorer OpenAPI declares only 200 responses, so the failure modes above are observed rather than specified.

## References

- Data model and composite keys: `data-model/gala-games-data-model.yml`
- Errors: `errors/gala-games-problem-types.yml`
- Event surface (websocket block streaming exists but is undocumented): `asyncapi/gala-games-event-surface.yml`
