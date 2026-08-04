# Vitality (vitality-uk)

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

Vitality is a United Kingdom health and life insurer, operating as VitalityHealth and VitalityLife under the Vitality umbrella brand and owned by the South African financial services group Discovery Limited. Formed from the 2004 PruHealth joint venture with Prudential and the 2010 acquisition of Standard Life Healthcare, and rebranded to Vitality in 2014, it is the UK's third-largest private medical insurer behind Bupa and AXA, with roughly 1.9 million members and a shared-value model that prices private medical insurance, life cover and protection against member health and activity engagement.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/vitality-uk/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/vitality-uk/refs/heads/main/apis.yml)

## Tags

- Insurance
- United Kingdom
- Health Insurance
- Life Insurance
- Employee Benefits
- Carrier
- Policy Administration
- Underwriting
- Partner Gated

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## APIs

**Vitality Partner API Gateway** — `https://apis.vitality.co.uk` (undocumented, partner-gated).

Vitality publishes **no public, self-serve developer portal and no downloadable API definitions**. This is a deliberate, verified record rather than a gap in research — see [`review.yml`](review.yml) for the full probe log.

What Vitality does have is a real but undocumented partner gateway, and — found on the 2026-07-25 enrichment round — a **fully published OpenID Connect identity layer**:

- `https://apis.vitality.co.uk/` returns **HTTP 200** with the WSO2 API Manager default landing page ("Welcome to APIM"). Response headers expose the origin host `wso2-prd-apigw.tvc.vitality.co.uk` on port 8243 behind an AWS load balancer in eu-west-1. A second edge, `m.apis.vitality.co.uk`, serves the identical page.
- The WSO2 developer-portal routes (`/devportal`, `/publisher`, `/store`, `/api/am/devportal/v3/apis`) and every spec path tried (`/openapi.json`, `/openapi.yaml`, `/swagger.json`, `/api-docs`) return the WSO2 404 fault document. The only reachable listing, `/services`, shows just the WSO2 sample services (`echo`, `Version`).
- **A complete OpenID Connect discovery document is served anonymously** at `/oauth2/token/.well-known/openid-configuration` and `/oauth2/oidcdiscovery/.well-known/openid-configuration` (both HTTP 200), with a live JWKS at `/oauth2/jwks`. An earlier round recorded "no openid-configuration" because it probed only the RFC 8615 host root — that path does 404, the WSO2 Identity Server paths do not. Captured verbatim in [`well-known/`](well-known/).
- `POST https://apis.vitality.co.uk/token` with `grant_type=client_credentials` returns **HTTP 401 `invalid_client`** — a live token endpoint that only accepts partner-issued credentials. Authorization, userinfo, introspection, revocation, device-authorization, check-session and logout endpoints are all live. Dynamic client registration is advertised in discovery but **404s through the public gateway** — registration is a commercial process, not self-serve.
- Additional login edges `ah-login.apis.vitality.co.uk` (adviser hub) and `eh-login.apis.vitality.co.uk` (employer hub), plus `pre.`, `test.` and `uat.` environments, all share the issuer `https://apis.vitality.co.uk/oauth2/token`.
- Vitality's own Workplace Connect adviser SPA is configured against a live **Sitecore Experience Edge GraphQL** endpoint at `https://cd.wc.vitality.co.uk/sitecore/api/graph/edge`; introspection is gated on an `sc_apikey`. It is a CMS content surface, not an insurance API.
- `developer.vitality.co.uk`, `developers.vitality.co.uk`, `docs.vitality.co.uk` and `api.vitality.co.uk` do not resolve in DNS. `www.vitality.co.uk` and its `/developers`, `/api`, `/partners` paths sit behind Cloudflare bot protection (HTTP 403).

### Artifacts

| Artifact | File |
|---|---|
| Authentication profile (OAuth2/OIDC, verbatim from discovery) | [`authentication/vitality-uk-authentication.yml`](authentication/vitality-uk-authentication.yml) |
| OAuth scopes (OIDC standard scopes only) | [`scopes/vitality-uk-scopes.yml`](scopes/vitality-uk-scopes.yml) |
| Well-known index + raw OIDC/JWKS documents | [`well-known/`](well-known/) |
| Conformance (what the identity layer does and does not implement) | [`conformance/vitality-uk-conformance.yml`](conformance/vitality-uk-conformance.yml) |
| Error catalogue (observed WSO2/OAuth envelopes) | [`errors/vitality-uk-problem-types.yml`](errors/vitality-uk-problem-types.yml) |
| API conventions (auth, tracing, error envelope, transport security) | [`conventions/vitality-uk-conventions.yml`](conventions/vitality-uk-conventions.yml) |
| Lifecycle (environment ladder, absences) | [`lifecycle/vitality-uk-lifecycle.yml`](lifecycle/vitality-uk-lifecycle.yml) |
| Domain security (TLS/HSTS/DNSSEC/CAA/SPF/DMARC) | [`security/vitality-uk-domain-security.yml`](security/vitality-uk-domain-security.yml) |
| llms.txt | [`llms/vitality-uk-llms.txt`](llms/vitality-uk-llms.txt) |

Vitality publicly described in August 2025 that it had deployed WSO2's open-source API Manager and integration tooling to decouple core services from front-end applications, cutting partner onboarding from six months to a few weeks. That is a **partner-integration programme, not a developer programme** — onboarding ends in issued client credentials, not a signup form.

### What is not exposed

- **Quote, bind, issue, FNOL** — none are publicly documented. All four run through consumer journeys on `vitality.co.uk`, the co-branded Lloyds Bank journey on `partners.vitality.co.uk`, adviser channels, and the member app.
- **ACORD posture** — *no ACORD reference found*. No mention of ACORD, AL3, ACORD XML, ACORD certification or NGDS on any reachable surface. Expected for a UK retail health-and-protection carrier: UK ACORD adoption is concentrated in the London subscription market (Blueprint Two / Velonetic CR&P, EBOT, ECOT), not in domestic PMI and life distribution.
- **Webhooks / events** — no event catalogue, no AsyncAPI.
- **Postman** — a public Postman search for "vitality insurance" returned zero workspaces and zero collections.
- **gRPC** — no published `.proto`. `/graphql` on the gateway returns 404 (the only GraphQL found is the Sitecore CMS edge above).
- **SDKs / packages** — no first-party client library on npm, PyPI or any other registry. The `@vitality-ds/*` npm scope is a design system published from `github.com/genie-engineering` and is **not** Vitality UK.
- **Status page, SLA, deprecation policy** — none. `status.vitality.co.uk`, `trust.vitality.co.uk` and `security.vitality.co.uk` do not resolve.
- **security.txt / vulnerability disclosure / trust centre** — none found. The gateway 404s `/.well-known/security.txt`; the corporate host is Cloudflare-gated, so absence there is unconfirmed rather than proven.
- **Open source** — the GitHub organisation [`VitalityUK`](https://github.com/VitalityUK) exists (created 2025-01-16) with zero public repositories.

### Naming cautions

- `api.vitality.io` is a **different, unrelated organisation** and must not be attributed to Vitality UK.
- Vitality Group / Vitality Global, the international wellness-programme arm of Discovery, is a separate entity from Vitality Health Limited and is out of scope for this record.

## Links

- [Website](https://www.vitality.co.uk/)
- [Contact](https://www.vitality.co.uk/contact/)
- [GitHub Organization](https://github.com/VitalityUK)
- [Review](review.yml)

## Maintainers

- Kin Lane — kin@apievangelist.com
