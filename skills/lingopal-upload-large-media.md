---
name: Upload large media to Lingopal
description: Move a large media or document file into Lingopal with a presigned multipart storage upload, then register it as a job.
api: openapi/lingopal-openapi-original.json
operations:
  - createStorageUpload
  - completeStorageUpload
  - registerJobs
  - getJob
generated: '2026-07-19'
method: generated
source: https://docs.lingopal.ai/guides/uploads/multipart-upload
---

# Upload large media to Lingopal

Use this path when `uploadAndRegisterJob` returns `413` — the file is over the direct
upload limit. The multipart flow is four steps and the upload itself does not go
through the Lingopal API.

## Steps

1. **Create the upload.** Call `createStorageUpload` (`POST /v2/storage/uploads`)
   with `file_ext` and `file_size`. The response carries `source_url`, `file_path`,
   `upload_id`, `upload_urls[]` (each with `part_number` and `upload_url`), plus the
   `method` and `headers` to use.

2. **Upload every part.** For each entry in `upload_urls`, send the corresponding
   byte range directly to `upload_url` using the returned `method` and `headers`.
   These are presigned storage URLs — do **not** attach the `X-API-Key` header to
   them. Capture the `ETag` returned for each part and pair it with its
   `part_number`.

3. **Complete the upload.** Call `completeStorageUpload`
   (`POST /v2/storage/uploads/complete`) with `file_path`, `upload_id`, and the
   `etags` array of `{part_number, etag}`. A `403` here means the upload cannot be
   completed — re-verify `upload_id`, `file_path`, and that every ETag matches the
   part it came from. The response returns the final `file_path` and `source_url`.

4. **Register the source.** Call `registerJobs` (`POST /v2/jobs/register`) with an
   `items[]` entry whose `source_url` is the one returned by step 3, plus `name` and
   `media_type`. Check `results[].status`, `registered_count` and `error_count` — a
   `200` does not guarantee every item registered. Keep the returned `job_id`; you
   can confirm registration metadata with `getJob` (`GET /v2/jobs/{job_id}`).

Once registered, continue with the translate-media or generate-subtitles skill.

## Rules

- **Never send the API key to a presigned URL.** `X-API-Key` belongs only on
  `api.lingopal.ai` calls.
- Do not reorder or skip parts; every `part_number` from step 1 must be uploaded and
  reported back in step 3.
- Part uploads are safely retryable — a re-uploaded part supersedes the prior attempt
  and yields a fresh ETag; use the latest one. The Lingopal API calls themselves have
  no idempotency key, so retry those only on transient `5xx`/network failures.
- Never auto-retry a `422`; read `detail[].loc` and correct the named field.

See `conventions/lingopal-conventions.yml` (uploads) and
`data-model/lingopal-data-model.yml` (StorageUpload → UploadPart → Job).
