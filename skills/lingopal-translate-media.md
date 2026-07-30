---
name: Translate and dub media with Lingopal
description: Register audio or video with Lingopal, run a dubbing/translation workflow into one or more languages, poll to completion, and download the transcript.
api: openapi/lingopal-openapi-original.json
operations:
  - listLanguages
  - registerJobs
  - uploadAndRegisterJob
  - translateJob
  - getJobStatus
  - getJob
  - downloadJobTranscript
  - exportJobTranscript
generated: '2026-07-19'
method: generated
source: https://docs.lingopal.ai/guides/translation/media
---

# Translate and dub media with Lingopal

Media translation is asynchronous. Register the source once to get a reusable
`job_id`, start a workflow, poll until it settles, then retrieve outputs.

## Before you start

- Authenticate with the `X-API-Key` header against `https://api.lingopal.ai`.
- Call `listLanguages` with `language_type = dubbing` and check `dubbing_support`
  (and `voice_cloning_support` if you need voice preservation) on each target locale
  before you submit a workflow.

## Steps

1. **Register the source.** Pick one:
   - Existing public URL — `registerJobs` (`POST /v2/jobs/register`) with `items[]`,
     each carrying `source_url`, `name`, `media_type`. This is the batch path.
   - Small local file — `uploadAndRegisterJob` (`POST /v2/jobs/upload-and-register`)
     with the multipart `file`, plus `name` and `media_type`. A `413` means the file
     exceeds the direct upload limit — switch to the multipart storage upload skill.

2. **Capture the `job_id`.** For `registerJobs`, the call can return HTTP `200` while
   individual items fail: inspect `results[].status`, `registered_count` and
   `error_count`, and keep each successful `job_id` independently. Do not treat the
   `200` as blanket success.

3. **Start the workflow.** Call `translateJob`
   (`POST /v2/jobs/{job_id}/translations`) with `target_languages`. Useful options:
   `context` (domain context), `remove_background_audio`, `number_of_speakers`,
   `enhance`, `burn_subtitles_on_source`, `prepend_ai_warning`,
   `experimental_force_accent`, and `reset`.

4. **Poll to completion.** Call `getJobStatus` (`GET /v2/jobs/{job_id}/status`).
   Top-level `status` is `queued`, `processing`, `completed` or `failed`. Also read
   `workflows.translation`, `stage`, `sub_stage`, `outputs`, `error` and `updated_at`.
   Poll every few seconds, lengthen the interval for long media, and stop at
   `completed` or `failed`. Use `getJob` (`GET /v2/jobs/{job_id}`) only for
   registration metadata and links — it is not the progress endpoint.

5. **Retrieve outputs.** Once `outputs` shows the transcript is available, call
   `downloadJobTranscript` (`GET /v2/jobs/{job_id}/outputs/transcript`) with
   `locale`, `layout` and `file_format`; or `exportJobTranscript`
   (`POST /v2/jobs/{job_id}/outputs/transcript`) when you need column selection via
   `include_columns` / `exclude_columns`.

## Rules

- **One workflow at a time per job.** Wait for the active workflow to finish before
  starting another run on the same `job_id`.
- **`reset` defaults to `true` on `translateJob`** — it prunes stale target-language
  state before refreshing progress. Set it explicitly when rerun behavior matters.
- **No idempotency key exists.** A re-sent `translateJob` starts real work again.
  Only retry transient `5xx`/network failures with backoff and jitter; never
  auto-retry a `422`.
- On `status: failed`, read `error` on the status response before starting a new
  workflow — do not blindly resubmit.

## Errors

`401` invalid key · `404` unknown `job_id` or output not ready · `413` file too large
for direct upload · `422` validation failure (inspect `detail[].loc`) · `500` retry
with backoff. See `errors/lingopal-problem-types.yml`.
