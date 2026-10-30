# Gala Games

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Gala (formerly Gala Games) is a Web3 gaming and entertainment company that operates GalaChain, a Hyperledger Fabric based Layer 1 built for games, music and film. Its public API surface is REST-first rather than RPC. The GalaChain Gateway publishes an OpenAPI document per deployed chaincode contract (token, DEX v3, launchpad, fee and public key) across eighteen channel/contract pairs including per-game channels such as mirandus, championsarena and thewalkingdeadempires. GalaConnect exposes token swaps, headless wallet creation and cross-chain operations at api-galaswap.gala.com, a Block Explorer API serves blocks, transactions and balances, and the Gala DeFi backend serves pool, trading, liquidity and leaderboard data for GalaSwap. Write operations are authenticated by secp256k1 request signatures rather than bearer tokens, and every write carries a uniqueKey that GalaChain enforces as an idempotency key.

**Areas:** Company · Blockchain · Web3 · Gaming · NFT · Tokens · DeFi · Cryptocurrency · Distributed Ledger · Smart Contracts · Entertainment · Decentralized Exchange

## Gala Games on APIs.io

Everything profiled for Gala Games is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/gala-games/](https://apis.io/providers/gala-games/)** — or go straight to one of the APIs or properties below.

### APIs (24)

- [Gala Games API: GalaChain Operations API](https://apis.io/apis/gala-games/gala-games-api-galachain-operations-api/) — These operations are direct interactions with the GalaChain blockchain, with the GalaConnect API doing little more than proxying your request.
- [Gala Games API: GalaConnect Operations API](https://apis.io/apis/gala-games/gala-games-api-galaconnect-operations-api/) — The API: GalaConnect Operations API from Gala Games — 11 operation(s) for api: galaconnect operations.
- [Gala Games Coin Gecko API](https://apis.io/apis/gala-games/gala-games-coin-gecko-api/) — The coin-gecko API from Gala Games — 6 operation(s) for coin-gecko.
- [Gala Games Competition API](https://apis.io/apis/gala-games/gala-games-competition-api/) — The competition API from Gala Games — 8 operation(s) for competition.
- [Gala Games Explore API](https://apis.io/apis/gala-games/gala-games-explore-api/) — The explore API from Gala Games — 6 operation(s) for explore.
- [Gala Games Explorer API](https://apis.io/apis/gala-games/gala-games-explorer-api/) — The Explorer API from Gala Games — 16 operation(s) for explorer.
- [Gala Games Extension Version API](https://apis.io/apis/gala-games/gala-games-extensionversion-api/) — The ExtensionVersion API from Gala Games — 1 operation(s) for extensionversion.
- [Gala Games GalaConnect Client Operations API](https://apis.io/apis/gala-games/gala-games-galaconnect-client-operations-api/) — These are operations that you should generally do via the GalaConnect website client. Nonetheless, they are documented here for completeness. These APIs are…
- [Gala Games Health API](https://apis.io/apis/gala-games/gala-games-health-api/) — The Health API from Gala Games — 1 operation(s) for health.
- [Gala Games Incentive API](https://apis.io/apis/gala-games/gala-games-incentive-api/) — The incentive API from Gala Games — 8 operation(s) for incentive.
- [Gala Games Leaderboard API](https://apis.io/apis/gala-games/gala-games-leaderboard-api/) — The leaderboard API from Gala Games — 4 operation(s) for leaderboard.
- [Gala Games Market API](https://apis.io/apis/gala-games/gala-games-market-api/) — The market API from Gala Games — 6 operation(s) for market.
- [Gala Games Marketplace API](https://apis.io/apis/gala-games/gala-games-marketplace-api/) — The marketplace API from Gala Games — 2 operation(s) for marketplace.
- [Gala Games Methods API](https://apis.io/apis/gala-games/gala-games-methods-api/) — The Methods API from Gala Games — 71 operation(s) for methods.
- [Gala Games Onboarding API](https://apis.io/apis/gala-games/gala-games-onboarding-api/) — The onboarding API from Gala Games — 1 operation(s) for onboarding.
- [Gala Games Pairs API](https://apis.io/apis/gala-games/gala-games-pairs-api/) — The pairs API from Gala Games — 2 operation(s) for pairs.
- [Gala Games Price Oracle API](https://apis.io/apis/gala-games/gala-games-price-oracle-api/) — The price-oracle API from Gala Games — 1 operation(s) for price-oracle.
- [Gala Games Protocol Fee Sweep API](https://apis.io/apis/gala-games/gala-games-protocol-fee-sweep-api/) — The protocol-fee-sweep API from Gala Games — 2 operation(s) for protocol-fee-sweep.
- [Gala Games Token Class API](https://apis.io/apis/gala-games/gala-games-token-class-api/) — The token-class API from Gala Games — 1 operation(s) for token-class.
- [Gala Games Token Registry API](https://apis.io/apis/gala-games/gala-games-token-registry-api/) — The token-registry API from Gala Games — 3 operation(s) for token-registry.
- [Gala Games Trade API](https://apis.io/apis/gala-games/gala-games-trade-api/) — The trade API from Gala Games — 20 operation(s) for trade.
- [Gala Games User API](https://apis.io/apis/gala-games/gala-games-user-api/) — The user API from Gala Games — 3 operation(s) for user.
- [Gala Games Wallet Extension Compat API](https://apis.io/apis/gala-games/gala-games-walletextensioncompat-api/) — The WalletExtensionCompat API from Gala Games — 52 operation(s) for walletextensioncompat.
- [Gala Games Whats New API](https://apis.io/apis/gala-games/gala-games-whats-new-api/) — The Whats New API from Gala Games — 1 operation(s) for whats new.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Gala Games Event Surface](https://apis.io/asyncapis/gala-games/gala-games-event-surface/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [gala-launchpad](https://apis.io/servers/gala-games/gala-launchpad/)

#### Overlays (8)

OpenAPI overlays, including conversational phrasing for each operation.

- [Gala Games Block Explorer Overlay](https://apis.io/overlays/gala-games/gala-games-block-explorer-overlay/)
- [Gala Games Defi Backend Overlay](https://apis.io/overlays/gala-games/gala-games-defi-backend-overlay/)
- [Gala Games Galachain Asset Dexv3 Contract Overlay](https://apis.io/overlays/gala-games/gala-games-galachain-asset-dexv3-contract-overlay/)
- [Gala Games Galachain Asset Fee Contract Overlay](https://apis.io/overlays/gala-games/gala-games-galachain-asset-fee-contract-overlay/)
- [Gala Games Galachain Asset Launchpad Contract Overlay](https://apis.io/overlays/gala-games/gala-games-galachain-asset-launchpad-contract-overlay/)
- [Gala Games Galachain Asset Public Key Contract Overlay](https://apis.io/overlays/gala-games/gala-games-galachain-asset-public-key-contract-overlay/)
- [Gala Games Galachain Asset Token Contract Overlay](https://apis.io/overlays/gala-games/gala-games-galachain-asset-token-contract-overlay/)
- [Gala Games Galaconnect Overlay](https://apis.io/overlays/gala-games/gala-games-galaconnect-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Gala Games Authentication](https://apis.io/security/gala-games/gala-games-authentication/)
- [Gala Games Domain Security](https://apis.io/security/gala-games/gala-games-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Gala Games Plans Pricing](https://apis.io/plans/gala-games/gala-games-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Gala Games Rate Limits](https://apis.io/rate-limits/gala-games/gala-games-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://gala.com/)
- [Documentation](https://docs.galachain.com/latest/)
- [Getting started](https://docs.galachain.com/latest/getting-started/)
- [API reference](https://gateway-mainnet.galachain.com/docs)
- [Sign up](https://games.gala.com/account)
- [Support](https://support.gala.com/hc/en-us)
- [Blog](https://connect.gala.com/blog/)
- [GitHub](https://github.com/GalaChain)

## API Evangelist rating

**Kin Score 41.5** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 70.0 |
| Contract quality | 48.2 |
| Contract governance | 4.5 |
| Operational transparency | 39.5 |
| Developer ergonomics | 73.2 |
| Access clarity | 0.0 |

**Agent readiness 35.0** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | — |

**Regulatory alignment 21.3** — Securities & Market Data

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/gala-games/](https://apis.io/providers/gala-games/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Gala Games: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/gala-games/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
