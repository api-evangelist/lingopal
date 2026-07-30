# Lingopal

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
