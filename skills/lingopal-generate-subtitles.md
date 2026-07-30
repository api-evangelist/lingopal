---
name: Generate subtitles with Lingopal
description: Generate subtitle tracks for a registered Lingopal media job and download the completed track.
api: openapi/lingopal-openapi-original.json
operations:
  - listLanguages
  - generateJobSubtitles
  - getJobStatus
  - downloadJobSubtitle
generated: '2026-07-19'
method: generated
source: https://docs.lingopal.ai/guides/subtitles/generate
---

# Generate subtitles with Lingopal

Subtitles are a workflow run against an already-registered media job. Register the
media first (see the translate-media skill) — this skill starts at the `job_id`.

## Steps

1. **Check target locales.** Call `listLanguages` with `language_type = text` and
   confirm `text_support` for each subtitle target language.

2. **Generate the tracks.** Call `generateJobSubtitles`
   (`POST /v2/jobs/{job_id}/subtitles`) with `target_languages`. Options:
   `format` (subtitle file format), `burn_subtitles`, `include_source_language`,
   `word_count_per_line`, and `reset`.

3. **Poll to completion.** Call `getJobStatus` (`GET /v2/jobs/{job_id}/status`) and
   watch `workflows.subtitles` alongside the top-level `status`. Each entry under
   `languages` carries `target_language` and its own `status`. Stop at `completed`
   or `failed`.

4. **Download the track.** Call `downloadJobSubtitle`
   (`GET /v2/jobs/{job_id}/outputs/subtitle`) with `file_type`, `locale` and
   `line_length`. A `404` means the requested locale's track is not ready or the
   `job_id` is wrong — re-check `outputs` on the status response first.

## Rules

- **`reset` defaults to `false` here**, unlike `translateJob` where it defaults to
  `true`. Set it explicitly whenever rerun behavior matters, so an unintended
  re-generation does not silently reuse or discard prior state.
- Wait for any active workflow on the job to finish before starting another.
- No idempotency key exists; only retry transient `5xx`/network failures with
  exponential backoff and jitter, and never auto-retry a `422`.
- Authenticate with `X-API-Key` on every call against `https://api.lingopal.ai`.

See `conventions/lingopal-conventions.yml` and `errors/lingopal-problem-types.yml`.
