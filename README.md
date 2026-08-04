# Lingopal

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

Lingopal is an AI translation company building a real-time language layer for live and
on-demand video — sub-10-second AI dubbing and captioning for livestreams across 100+
languages, with voice cloning to preserve speaker characteristics. It serves sports and
news broadcasters, streaming and FAST channel operators, live events, faith
organizations, and contact centers.

Lingopal publishes a public **v2 REST API** documented at
[docs.lingopal.ai](https://docs.lingopal.ai/) with a machine-readable OpenAPI 3.1
specification, authenticated with an `X-API-Key` header. Text translation is synchronous;
media, document, and subtitle workflows are asynchronous and polled through a job model.

- Website — https://lingopal.ai/
- Documentation — https://docs.lingopal.ai/
- API reference — https://docs.lingopal.ai/reference
- OpenAPI — https://docs.lingopal.ai/openapi/v2.json
- GitHub — https://github.com/lingopal-ai
- Pricing — https://lingopal.ai/pricing

Backed by: dcm-ventures

## Artifacts in this repo

| Artifact | Path | Method |
| --- | --- | --- |
| OpenAPI 3.1 (13 operations) | `openapi/lingopal-openapi-original.json` | searched |
| llms.txt | `llms/lingopal-llms.txt` | searched |
| Authentication profile | `authentication/lingopal-authentication.yml` | searched |
| API conventions | `conventions/lingopal-conventions.yml` | searched |
| Error catalog | `errors/lingopal-problem-types.yml` | searched |
| Lifecycle / versioning | `lifecycle/lingopal-lifecycle.yml` | searched |
| Conformance | `conformance/lingopal-conformance.yml` | derived |
| Data model / ERD | `data-model/lingopal-data-model.yml` | derived |
| MCP tool surface (candidate) | `mcp/lingopal-mcp.yml` | derived |
| OpenAPI Overlay | `overlays/lingopal-v2-overlay.yaml` | generated |
| Agent Skills (4) | `skills/` | generated |
| Agentic access contracts | `agentic-access/lingopal-agentic-access.yml` | generated |
| Domain security probe | `security/lingopal-domain-security.yml` | probed |
| Well-known probe (no hits) | `well-known/lingopal-well-known.yml` | probed |

## Not published by Lingopal

No SDKs or client libraries in any package registry, no CLI, no embedded UI components,
no webhooks or AsyncAPI event surface, no OAuth scopes (API-key auth only), no sandbox or
test-mode credentials, no dated changelog, no status page, no deprecation policy, no
`/.well-known/` documents including `security.txt`, no vulnerability-disclosure program,
and no trust center or named compliance certifications. These are recorded as honest
negatives rather than fabricated artifacts.

## Known issue to report upstream

The OpenAPI `servers[]` declares `https://vod-api.lingopal-dev.com`, while every code
sample in the guides calls `https://api.lingopal.ai`. Both hosts respond; the documented
production host is `api.lingopal.ai`. Captured in `overlays/` and `lifecycle/`.
