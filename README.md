# Voltus (voltus)

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

Voltus is a United States virtual power plant (VPP) operator and distributed energy resource (DER) technology platform, headquartered in San Francisco, California, that aggregates commercial, industrial, residential and transportation loads and batteries into wholesale electricity markets across all of North America's organized markets (AESO, CAISO, ERCOT, IESO, ISO-NE, MISO, NYISO, PJM, SPP). It sits in the demand-response and flexibility layer of the energy value chain: it is not a utility and not a metering data holder, so no Green Button, Consumer Data Right or smart-meter data mandate applies to it — mandate regime is honestly none. Its API posture is unusually good for the sector and unusually split. Voltus runs a genuine public developer portal at api.voltus.co/docs (Docusaurus, no login) with concepts, tutorials, an OpenAPI-generated reference and a changelog, plus a fully anonymous sandbox at sandbox.voltus.co that answers real HTTP requests with the documented public key `X-Voltus-API-Key: secret` — a developer can call it before signing anything. Production, however, is partner-only: `api.voltus.co/2022-04-15` returns 401 Permission denied to the sandbox key, and real access requires a commercial partnership plus a signed Letter of Authorization per site. Site telemetry and dispatch control are exposed to partners over that account-scoped REST API and over OpenADR 2.0a Simple HTTP PULL with mutual TLS; Voltus publishes no open grid or market data of its own, so consumer/site energy data is available under contract while market data is closed. No downloadable OpenAPI or Swagger document is served — `/openapi.json`, `/swagger.json` and `/openapi3.yaml` all 404.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/voltus/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/voltus/refs/heads/main/apis.yml)

## Tags

- Energy
- United States
- Electricity
- Demand Response
- Virtual Power Plant
- DER
- Grid
- Energy Markets
- Flexibility
- Energy Storage
- OpenADR
- Telemetry

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### Voltus Dispatches API

Grid dispatch events for enrolled sites — list dispatches, retrieve a single dispatch by ID, and create a test dispatch. Documented operations are `GET /dispatches`, `POST /dispatches` and `GET /dispatch`. Dispatch objects carry program, market (e.g. MISO), program_type, start/end time, modification number and per-site drop_by and commitment values. Verified live against the public sandbox.

- **Human URL:** [https://api.voltus.co/docs/openapi/dispatches](https://api.voltus.co/docs/openapi/dispatches)
- **Base URL:** `https://api.voltus.co/2022-04-15`

#### Tags

- Demand Response
- Dispatch
- Energy Markets

#### Properties

- [API Reference](https://api.voltus.co/docs/openapi/dispatches)
- [Documentation](https://api.voltus.co/docs/concepts/demand_response)
- [Tutorials](https://api.voltus.co/docs/tutorials/create-a-dispatch-integration)
- [Tutorials](https://api.voltus.co/docs/tutorials/get-sandbox-dispatches)
- [Tutorials](https://api.voltus.co/docs/tutorials/test-your-dispatch-integration)
- [Sandbox](https://api.voltus.co/docs/concepts/public-credentials)
- [Authentication](https://api.voltus.co/docs/openapi/voltus-api-reference)
- [ChangeLog](https://api.voltus.co/docs/changelog)

### Voltus Sites API

Read the sites (facilities) enrolled with Voltus under the calling account, with their meters. The single documented operation is `GET /sites`, returning site name, id and customer_location_id, with an `asset_type` field on each meter object added 2026-07-01. Verified live against the public sandbox.

- **Human URL:** [https://api.voltus.co/docs/openapi/sites](https://api.voltus.co/docs/openapi/sites)
- **Base URL:** `https://api.voltus.co/2022-04-15`

#### Tags

- Sites
- Metering
- DER

#### Properties

- [API Reference](https://api.voltus.co/docs/openapi/sites)
- [Tutorials](https://api.voltus.co/docs/tutorials/first-api-request-get-sites)
- [Sandbox](https://api.voltus.co/docs/concepts/public-credentials)
- [ChangeLog](https://api.voltus.co/docs/changelog/meter-asset-type)

### Voltus Telemetry API

Submit and retrieve interval energy telemetry for enrolled sites. Documented operations are `POST /telemetry`, `POST /telemetry/controllable-load` and `GET /telemetry/kw` (maximum 10 sites per request, maximum 90-day interval, 10,000 data points per site). This is the site-level customer energy-usage surface — account-scoped to the API key holder, not a Green Button or Consumer Data Right style consented third-party feed.

- **Human URL:** [https://api.voltus.co/docs/openapi/telemetry](https://api.voltus.co/docs/openapi/telemetry)
- **Base URL:** `https://api.voltus.co/2022-04-15`

#### Tags

- Telemetry
- Metering
- Electricity
- Flexibility

#### Properties

- [API Reference](https://api.voltus.co/docs/openapi/telemetry)
- [Tutorials](https://api.voltus.co/docs/tutorials/get-telemetry)
- [Tutorials](https://api.voltus.co/docs/tutorials/send-telemetry)
- [Sandbox](https://api.voltus.co/docs/concepts/public-credentials)
- [ChangeLog](https://api.voltus.co/docs/changelog/telemetry-controllable-load)

### Voltus Webhooks API

Register HTTP callbacks so Voltus can push dispatch notifications instead of the partner polling. Documented operations are `GET /webhooks`, `POST /webhooks` and `DELETE /webhooks/{id}`.

- **Human URL:** [https://api.voltus.co/docs/openapi/webhooks](https://api.voltus.co/docs/openapi/webhooks)
- **Base URL:** `https://api.voltus.co/2022-04-15`

#### Tags

- Webhooks
- Dispatch
- Notifications

#### Properties

- [API Reference](https://api.voltus.co/docs/openapi/webhooks)
- [Webhooks](https://api.voltus.co/docs/openapi/webhooks)
- [Documentation](https://api.voltus.co/docs/tutorials/create-a-dispatch-integration)

### Voltus OpenADR 2.0a VTN

Standards-based alternative to the REST dispatch integration. Voltus states it "supports OpenADR2.0a via Simple HTTP (PULL)" and that "Voltus provides a VTN, and our partners run one or more VENs". Partners poll `https://openadr.voltus.co/vtn/YOUR_VEN_ID/OpenADR2/Simple/EiEvent` (recommended once per minute) and reply with an `oadrCreatedEvent` acknowledgement. Authentication is mutual TLS — the partner submits a Certificate Signing Request to Voltus and presents the returned client certificate.

- **Human URL:** [https://api.voltus.co/docs/concepts/openadr](https://api.voltus.co/docs/concepts/openadr)
- **Base URL:** `https://openadr.voltus.co`

#### Tags

- OpenADR
- Demand Response
- Standards
- Dispatch

#### Properties

- [Documentation](https://api.voltus.co/docs/concepts/openadr)
- [Tutorials](https://api.voltus.co/docs/tutorials/openadr)
- [ChangeLog](https://api.voltus.co/docs/changelog/sandbox-openadr-support)

## Common Properties

- [Website](https://www.voltus.co/)
- [Developer Portal](https://api.voltus.co/docs/)
- [Documentation](https://api.voltus.co/docs/)
- [API Reference](https://api.voltus.co/docs/openapi/voltus-api-reference)
- [Getting Started](https://api.voltus.co/docs/tutorials/first-api-request-get-sites)
- [Tutorials](https://api.voltus.co/docs/tutorials/)
- [Sandbox](https://api.voltus.co/docs/concepts/public-credentials)
- [Authentication](https://api.voltus.co/docs/openapi/voltus-api-reference)
- [ChangeLog](https://api.voltus.co/docs/changelog)
- [Support](mailto:api-support@voltus.co)
- [Blog](https://www.voltus.co/blog)
- [Terms of Service](https://www.voltus.co/legal/terms-of-service)
- [Privacy Policy](https://www.voltus.co/legal/privacy-policy)
- [GitHub Organization](https://github.com/voltus)
- [LinkedIn](https://www.linkedin.com/company/voltus-inc./)

## Maintainers

- Kin Lane — kin@apievangelist.com
