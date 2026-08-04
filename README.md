# Memberful (memberful)

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Memberful is a membership and subscription platform - owned by Patreon - that lets independent publishers, educators, and creators sell memberships, subscriptions, digital downloads, podcasts, and courses on their own site while Memberful handles checkout, recurring billing (through Stripe), and member management. Its public developer surface is a **native GraphQL API**, served per account at `POST https://ACCOUNT.memberful.com/api/graphql` and authenticated with an API key (bearer token) or an OAuth 2.0 access token. Memberful also offers OAuth 2.0 single sign-on for apps and HMAC-signed HTTP webhooks.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/memberful/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/memberful/refs/heads/main/apis.yml)

## Access model

Memberful prices the **platform**, not the API: the GraphQL API, OAuth SSO, and webhooks are included with a paid Memberful plan at no separate API fee. You create an API key by adding a Custom Application under Settings → Custom applications. The API is a single per-account GraphQL endpoint - there is no REST surface and no WebSocket. The always-current schema is the live **GraphQL API Explorer** in the dashboard; the schema and queries in this repo are honestly modeled from Memberful's public documentation and confirmed object descriptions, and modeled fields should be verified against that Explorer.

## Tags

- Memberships
- Subscriptions
- Payments
- Creators
- GraphQL
- Patreon

## Timestamps

- **Created:** 2026-07-05
- **Modified:** 2026-07-05

## APIs

### Memberful Members API

Query members and their profiles, emails, subscriptions, downloads, and custom JSON metadata (up to 50 keys), and create, update, or delete members through GraphQL mutations. Uses cursor-based pagination.

- **Human URL:** [https://memberful.com/help/custom-development-and-api/memberful-api/](https://memberful.com/help/custom-development-and-api/memberful-api/)
- **Base URL:** `https://ACCOUNT.memberful.com/api/graphql`

#### Properties

- [Documentation](https://memberful.com/help/custom-development-and-api/memberful-api/)
- [API Reference](https://memberful.com/docs/api-reference/memberful-api)
- [GraphQL Schema](graphql/memberful-schema.graphql)
- [GraphQL Guide](graphql/memberful-graphql.md)
- [Postman Collection](collections/memberful.postman_collection.json)
- [Open Collection](collections/memberful.opencollection.json)

### Memberful Subscriptions API

Read and manage member subscriptions - the link between a member and the pass/plan they pay for - including status, current period, trial state, activation and expiration, and the associated plan pricing.

- **Human URL:** [https://memberful.com/help/custom-development-and-api/memberful-api/](https://memberful.com/help/custom-development-and-api/memberful-api/)
- **Base URL:** `https://ACCOUNT.memberful.com/api/graphql`

### Memberful Plans and Passes API

Query the passes members subscribe to (called "Plans" in the dashboard) and the plans (pricing options such as monthly or annual) within each pass, including price, interval, and label. Coupons are also exposed here.

- **Human URL:** [https://memberful.com/help/custom-development-and-api/memberful-api/](https://memberful.com/help/custom-development-and-api/memberful-api/)
- **Base URL:** `https://ACCOUNT.memberful.com/api/graphql`

### Memberful Orders API

Query orders (transaction records) for a member or account, including totals, status, coupons applied, and the member and plan involved. Orders back the `order.purchased`, `order.completed`, `order.refunded`, and `order.suspended` webhook events.

- **Human URL:** [https://memberful.com/help/custom-development-and-api/memberful-api/](https://memberful.com/help/custom-development-and-api/memberful-api/)
- **Base URL:** `https://ACCOUNT.memberful.com/api/graphql`

### Memberful OAuth SSO API

OAuth 2.0 Authorization Code flow (with PKCE) for signing members into external apps. Authorize at `/oauth`, exchange the code at `/oauth/token`, then query the signed-in member at `/api/graphql/member`. Access tokens expire in 15 minutes; refresh tokens last one year. Scope is accepted but ignored.

- **Human URL:** [https://memberful.com/help/custom-development-and-api/sign-in-for-apps-via-oauth/](https://memberful.com/help/custom-development-and-api/sign-in-for-apps-via-oauth/)
- **Base URL:** `https://ACCOUNT.memberful.com`

## Webhooks

Memberful delivers events as HMAC-SHA256-signed HTTP POSTs (JSON body, `X-Memberful-Webhook-Signature` header) - not a WebSocket. Documented event identifiers include `member_signup`, `member_updated`, `member.deleted`, `subscription.created`, `subscription.activated`, `subscription.deactivated`, `subscription.renewed`, `subscription.deleted`, `subscription_plan.created`, `subscription_plan.updated`, `subscription_plan.deleted`, `order.purchased`, `order.completed`, `order.refunded`, `order.suspended`, and `download.created`.

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/memberful)
- [Website](https://memberful.com)
- [Documentation](https://memberful.com/help/custom-development-and-api/)
- [GitHub Organization](https://github.com/memberful)
- [Plans](plans/memberful-plans-pricing.yml)
- [Rate Limits](rate-limits/memberful-rate-limits.yml)
- [Fin Ops](finops/memberful-finops.yml)

## Pricing

Memberful's published plan is **Standard at $49/month plus a 4.9% Memberful transaction fee**, with unlimited members and the full feature set; **Enterprise** is custom-priced with dedicated support and migration help. Stripe charges its own ~2.9% + $0.30 per transaction for payment processing, separately from Memberful's fee. Third-party aggregators also report Free/Starter (10% fee), Pro ($25/mo), and Premium ($100/mo) tiers, which are not confirmed on Memberful's current pricing page and are flagged as modeled in the plans file.

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
