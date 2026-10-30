# GroundTruth

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

GroundTruth is a location-intelligence performance advertising platform (formerly xAd) that helps brands and agencies plan, launch, and measure omnichannel ad campaigns tied to real-world behavior. Its Ads Manager, proprietary Blueprints location-mapping technology, and Dynamic Intent Prediction AI turn visitation, behavioral, and demographic signals into targeting and store-visit and sales attribution across verticals including QSR/restaurants, CPG, auto, retail, healthcare, education, travel, and political. GroundTruth is backed by Emergence Capital and IVP and became part of ZeroToOne.AI in 2026. GroundTruth publishes two public REST API surfaces: the Ads Manager Public API at api-public.groundtruth.com (259 operations across accounts, organizations, tenants, campaigns, ad groups, creatives, audiences, jobs, uploads, search and reporting, with a live OpenAPI 3.1 document and Swagger UI at /docs), and the Groundtruth Reporting API at reporting.groundtruth.com (59 read-only demand-reporting operations described by an OpenAPI 3.0.1 document). Both authenticate with the paired X-GT-USER-ID and X-GT-API-KEY headers; credentials are issued on request rather than self-serve.

**Areas:** Company · MarTech · Advertising · Location Intelligence · Marketing · AdTech · Location-Based Marketing · Advertising API · Campaign Management · Ad Reporting · Attribution · Geofencing

## GroundTruth on APIs.io

Everything profiled for GroundTruth is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/groundtruth/](https://apis.io/providers/groundtruth/)** — or go straight to one of the APIs or properties below.

### APIs (19)

- [GroundTruth Documentation MCP Server](https://apis.io/apis/groundtruth/groundtruth-documentation-mcp-server/) — Model Context Protocol endpoint served on the GroundTruth developer documentation host. It answers MCP JSON-RPC on /mcp but returns error -32001…
- [GroundTruth Accounts API](https://apis.io/apis/groundtruth/groundtruth-accounts-api/) — The Accounts API from GroundTruth — 6 operation(s) for accounts.
- [GroundTruth Ad Groups API](https://apis.io/apis/groundtruth/groundtruth-ad-groups-api/) — The Ad Groups API from GroundTruth — 6 operation(s) for ad groups.
- [GroundTruth Audiences API](https://apis.io/apis/groundtruth/groundtruth-audiences-api/) — The Audiences API from GroundTruth — 2 operation(s) for audiences.
- [GroundTruth Campaigns API](https://apis.io/apis/groundtruth/groundtruth-campaigns-api/) — The Campaigns API from GroundTruth — 6 operation(s) for campaigns.
- [GroundTruth Creative Assets API](https://apis.io/apis/groundtruth/groundtruth-creative-assets-api/) — The Creative Assets API from GroundTruth — 2 operation(s) for creative assets.
- [GroundTruth Creatives API](https://apis.io/apis/groundtruth/groundtruth-creatives-api/) — The Creatives API from GroundTruth — 5 operation(s) for creatives.
- [GroundTruth Demand API](https://apis.io/apis/groundtruth/groundtruth-demand-api/) — Demand API documentation
- [GroundTruth Direct Mail API](https://apis.io/apis/groundtruth/groundtruth-direct-mail-api/) — The Direct Mail API from GroundTruth — 1 operation(s) for direct mail.
- [GroundTruth Health API](https://apis.io/apis/groundtruth/groundtruth-health-api/) — The Health API from GroundTruth — 1 operation(s) for health.
- [GroundTruth Jobs API](https://apis.io/apis/groundtruth/groundtruth-jobs-api/) — The Jobs API from GroundTruth — 9 operation(s) for jobs.
- [GroundTruth Miscellaneous API](https://apis.io/apis/groundtruth/groundtruth-miscellaneous-api/) — The Miscellaneous API from GroundTruth — 1 operation(s) for miscellaneous.
- [GroundTruth Organizations API](https://apis.io/apis/groundtruth/groundtruth-organizations-api/) — The Organizations API from GroundTruth — 2 operation(s) for organizations.
- [GroundTruth Reporting API](https://apis.io/apis/groundtruth/groundtruth-reporting-api/) — The Reporting API from GroundTruth — 160 operation(s) for reporting.
- [GroundTruth Search API](https://apis.io/apis/groundtruth/groundtruth-search-api/) — The Search API from GroundTruth — 18 operation(s) for search.
- [GroundTruth Static API](https://apis.io/apis/groundtruth/groundtruth-static-api/) — The Static API from GroundTruth — 12 operation(s) for static.
- [GroundTruth Tenants API](https://apis.io/apis/groundtruth/groundtruth-tenants-api/) — The Tenants API from GroundTruth — 1 operation(s) for tenants.
- [GroundTruth Upload API](https://apis.io/apis/groundtruth/groundtruth-upload-api/) — The Upload API from GroundTruth — 1 operation(s) for upload.
- [GroundTruth Users API](https://apis.io/apis/groundtruth/groundtruth-users-api/) — The Users API from GroundTruth — 4 operation(s) for users.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [groundtruth-docs](https://apis.io/servers/groundtruth/groundtruth-docs/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Groundtruth Agentic Access](https://apis.io/agentic-access/groundtruth/groundtruth-agentic-access/)

#### Collections (2)

Postman and Open Collection request collections.

- [Ads Manager API](https://apis.io/collections/groundtruth/open-groundtruth-ads-manager/)
- [Groundtruth Reporting API](https://apis.io/collections/groundtruth/open-groundtruth-reporting/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Groundtruth Ads Manager Overlay](https://apis.io/overlays/groundtruth/groundtruth-ads-manager-overlay/)
- [Groundtruth Reporting Overlay](https://apis.io/overlays/groundtruth/groundtruth-reporting-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Groundtruth Authentication](https://apis.io/security/groundtruth/groundtruth-authentication/)
- [Groundtruth Domain Security](https://apis.io/security/groundtruth/groundtruth-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Groundtruth Plans Pricing](https://apis.io/plans/groundtruth/groundtruth-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Groundtruth Rate Limits](https://apis.io/rate-limits/groundtruth/groundtruth-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.groundtruth.com)
- [Documentation](https://api-docs.groundtruth.com/)
- [Getting started](https://www.groundtruth.com/get-started/)
- [API reference](https://api-docs.groundtruth.com/)
- [Sign up](https://ads.groundtruth.com/login?sign_up=1)
- [Support](https://help.groundtruth.com/hc/en-us)
- [Blog](https://www.groundtruth.com/insight/category/blog/)
- [Terms of service](https://www.groundtruth.com/terms-of-use/)
- [Privacy policy](https://www.groundtruth.com/privacy-policy/)

## API Evangelist rating

**Kin Score 38.8** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 76.7 |
| Contract quality | 46.1 |
| Contract governance | 4.5 |
| Operational transparency | 0.0 |
| Developer ergonomics | 57.7 |
| Access clarity | 34.2 |

**Agent readiness 31.6** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 24.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/groundtruth/](https://apis.io/providers/groundtruth/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for GroundTruth: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/groundtruth/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
