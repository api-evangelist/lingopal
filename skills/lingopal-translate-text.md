---
name: Translate text with Lingopal
description: Translate up to 100 strings synchronously with Lingopal, after confirming the target locale is supported.
api: openapi/lingopal-openapi-original.json
operations:
  - listLanguages
  - translateText
generated: '2026-07-19'
method: generated
source: https://docs.lingopal.ai/guides/translation/text
---

# Translate text with Lingopal

Text translation is the one synchronous surface in the Lingopal API. It returns
translations in the response — no job is created and nothing is persisted.

## Before you start

- Authenticate every request with the `X-API-Key` header. Lingopal does not accept
  bearer tokens. A `401` means the header is missing, misspelled, or the key is revoked.
- Base URL is `https://api.lingopal.ai`. All paths are versioned under `/v2`.

## Steps

1. **Confirm the locale is supported.** Call `listLanguages` with
   `language_type = text` (`GET /v2/languages/text`). Read the returned `locale`
   values — they are BCP 47 style, such as `es` or `pt-BR`. A locale that supports
   dubbing does not necessarily support text, so never assume; check `text_support`
   on the language entry.

2. **Submit the strings.** Call `translateText` (`POST /v2/translate/text`) with
   `texts` (up to 100 strings) and `target_language`. Optional fields:
   - `source_language` — omit to let Lingopal detect it.
   - `context` — domain or subject context that improves word choice.
   - `deduct_credits` — controls whether the call consumes plan credits.

3. **Read the result.** The response carries `workflow`, `status`, and a
   `translations[]` array where each entry has `source_text`, `translated_text` and
   `target_language`. Match results back to inputs by position and `source_text`.

## Rules

- **Do not batch beyond 100 strings** in one call — split into multiple requests.
- **There is no idempotency key.** Retries of a successful call re-translate and may
  re-consume credits. Only retry on transient `5xx` or network failures, with
  exponential backoff and jitter.
- **Never auto-retry a `422`.** Read `detail[]`, use each entry's `loc` to find the
  offending field and `msg` to explain it, then fix the request.
- Keep the API key server-side. Never place it in browser code or commit it.

## Errors

| Status | Meaning | Action |
| --- | --- | --- |
| 401 | Missing or invalid API key | Check the `X-API-Key` header and key status |
| 422 | Request validation failed | Inspect `detail[]`, correct the named field |
| 500 | Server failure | Retry with backoff; contact support if persistent |

See `errors/lingopal-problem-types.yml` and `conventions/lingopal-conventions.yml`.
