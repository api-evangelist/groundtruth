---
name: groundtruth-bulk-jobs-and-uploads
description: Run GroundTruth bulk campaign uploads, bulk downloads and location-filter uploads through the asynchronous Jobs surface — submit, poll, confirm or cancel.
api: openapi/groundtruth-ads-manager-openapi.yml
base_url: https://api-public.groundtruth.com
generated: '2026-08-12'
method: generated
source: openapi/groundtruth-ads-manager-openapi.yml (every operationId below verified present in the live spec)
operations:
  - upload
  - create_campaign_bulk_upload_job
  - confirm_campaign_bulk_upload_job
  - cancel_campaign_bulk_upload_job
  - create_campaign_bulk_download_job
  - create_location_filter_bulk_upload_job
  - confirm_location_filter_bulk_upload_job
  - cancel_location_filter_bulk_upload_job
  - get_jobs
  - get_job
  - export_campaigns_dashboard
  - download_location_filters
---

# Run bulk work on GroundTruth

GroundTruth has **no webhooks and no event stream**. Everything asynchronous is submit-then-poll
through the Jobs surface. This skill is the polling contract.

## Before you start

Both auth headers on every call, plus `tenant_id`:

```
X-GT-USER-ID: <your user id>
X-GT-API-KEY: <your api key>
```

## The pattern

Every bulk flow is the same three or four beats.

1. **Upload the file.** `upload` (POST `/upload`) puts the file where the platform can see it.
2. **Create the job.** `create_campaign_bulk_upload_job` (POST `/jobs/campaign_bulk_upload`) or
   `create_location_filter_bulk_upload_job` (POST `/jobs/location_filter_bulk_upload`). You get back
   a `job_id`.
3. **Poll.** `get_job` (GET `/jobs/{job_id}`) until it reaches a terminal state. `JobModel` carries
   back-references to `account_id`, `campaign_id`, `adgroup_id` and `creative_id`, so a completed
   job tells you what it actually touched. Use `get_jobs` (GET `/jobs`) to list jobs for the tenant.
4. **Commit or abandon.** Bulk *uploads* are two-phase — the job validates first and then waits for
   you:
   - `confirm_campaign_bulk_upload_job` (POST `/jobs/campaign_bulk_upload/{job_id}/confirm`) applies it.
   - `cancel_campaign_bulk_upload_job` (POST `/jobs/campaign_bulk_upload/{job_id}/cancel`) discards it.
   - The location-filter flow has the matching `confirm_location_filter_bulk_upload_job` and
     `cancel_location_filter_bulk_upload_job`.

   **Do not skip the confirm.** A created-but-unconfirmed upload job has not changed anything.

Bulk *downloads* are one-phase: `create_campaign_bulk_download_job`
(POST `/jobs/campaign_bulk_download`), then poll `get_job` for the result.

## Polling discipline

There is no published rate limit, no `429` response declared on any operation, and no
`Retry-After` header. That is not permission to poll hard — it means you have no signal when you
are being throttled. Poll with backoff (e.g. 2s, then doubling to a 30s ceiling) and stop after a
bounded number of attempts rather than looping forever.

## Related exports

Some exports do not go through Jobs at all and return the file directly:

- `export_campaigns_dashboard` (POST `/campaigns/dashboard/export`) — campaigns in a tenant as an
  Excel file.
- `download_location_filters` — location filters for an ad group.
- `get_account_pixels_export` — a spreadsheet of an account's pixels.

These return `text/html` or a binary body rather than `application/json` on success; do not parse
them as JSON.

## Handling errors

Standard envelope: `{"errors":[{"code","message","fields"}]}`. `400` means validation — read
`errors[].fields`. `500` means retry with backoff and capture the `x-gt-trace-id` response header,
which is the only correlation handle the API gives you and is **not** documented in the spec.

## Do not

- Do not re-submit a job on timeout — there is no idempotency key, so you may create a duplicate.
  Call `get_jobs` and look for an existing job first.
- Do not expect a callback. There are no webhooks, no AsyncAPI and no event surface anywhere in the
  GroundTruth contract; polling is the only mechanism.
