---
name: groundtruth-launch-campaign
description: Create a GroundTruth campaign with its ad groups and creatives, from the account hierarchy down to a live flight, using the Ads Manager Public API.
api: openapi/groundtruth-ads-manager-openapi.yml
base_url: https://api-public.groundtruth.com
generated: '2026-08-12'
method: generated
source: openapi/groundtruth-ads-manager-openapi.yml (every operationId below verified present in the live spec)
operations:
  - get_accounts
  - get_account
  - get_account_verticals
  - get_dmas
  - get_iab_categories
  - create_campaign
  - create_campaign_and_adgroups
  - get_campaign
  - create_adgroup
  - get_adgroup
  - get_adgroup_avails
  - create_creative
  - get_creative_preview
  - update_campaign
---

# Launch a GroundTruth campaign

Use this to stand up a new advertising flight on the GroundTruth Ads Manager platform.

## Before you start

**Authentication.** Every call needs **both** headers together. Neither works alone.

```
X-GT-USER-ID: <your user id>
X-GT-API-KEY: <your api key>
```

Credentials are issued on request — there is no self-serve key. A missing or partial header pair
returns `401` with `{"errors":[{"code":"UNAUTHENTICATED","message":"Sorry, you need to be
authenticated to perform this operation."}]}`.

**Tenant scoping.** `tenant_id` is a **required query parameter on almost every operation** (242 of
259). Resolve it once and carry it on every request. Most write operations also need
`organization_id` and/or `account_id`. A missing `tenant_id` is the most common cause of a `400`.

**There is no idempotency key.** Nothing in this API is safe to blind-retry. If `create_campaign`
times out, do **not** re-POST — call `get_campaigns` and check whether the campaign already exists
before trying again. See `conventions/groundtruth-conventions.yml`.

## Steps

1. **Find the account you are advertising for.**
   Call `get_accounts` with `tenant_id` and `organization_id`. It is paginated: `limit` (default 10,
   max 100), `page_num` (1-indexed). Read `has_next_page` from the response envelope to decide
   whether to keep going. Use `updated_since` (unix timestamp) if you only want recently changed
   accounts. Confirm the one you want with `get_account`.

2. **Pull the reference vocabularies you need.**
   These eight `/static/*` lookups need **no authentication**, so you can cache them ahead of time:
   `get_account_verticals`, `get_dmas`, `get_iab_categories`, `get_publisher_categories`,
   `get_audio_podcast_topics`, `get_audio_streaming_genres`,
   `get_streaming_video_content_categories`, `get_streaming_video_premium_ctv_categories`.
   Campaign and ad group payloads reference these ids — do not invent category or DMA values.

3. **Create the campaign.**
   Either `create_campaign` (POST `/campaigns`) on its own, or `create_campaign_and_adgroups`
   (POST `/campaigns/with_adgroups`) to create the campaign and its ad groups in one request. Prefer
   the combined operation when you already know the ad group structure — it is one write instead of
   several, which matters because there is no idempotency key to make a partial failure recoverable.
   `CampaignModel` has 34 fields; read the schema in the spec rather than guessing which are required.

4. **Verify the campaign landed.**
   `get_campaign` with the returned `campaign_id`. Do this before creating children — it is the only
   way to confirm the write, since there is no idempotent replay.

5. **Create ad groups.**
   `create_adgroup` (POST `/adgroups`) per ad group, or skip this if you used
   `create_campaign_and_adgroups`. `AdgroupModel` is the widest object in the API — 68 fields
   covering targeting, budget, flight dates, media type and placement. For digital-out-of-home use
   `create_dooh_adgroups`.

6. **Check inventory availability.**
   `get_adgroup_avails` (GET `/adgroups/{adgroup_id}/avails`) tells you what inventory the ad group's
   targeting can actually reach. Do this before attaching creatives, not after.

7. **Attach creatives.**
   `create_creative` (POST `/creatives`). Preview with `get_creative_preview` and, for click-through
   destinations, `get_creative_landing_page_preview`.

8. **Activate.**
   `update_campaign` (PUT `/campaigns/{campaign_id}`) to move the campaign into its live state.
   `patch_adgroup` (PATCH `/adgroups/{adgroup_id}`) for partial ad group edits — use PATCH rather
   than PUT when you only want to change a few of the 68 fields.

## Handling errors

| Status | Meaning | What to do |
|---|---|---|
| 400 | Validation failed | Read `errors[].fields` — it names the offending parameters. Check `tenant_id` first. |
| 401 | Not authenticated | Send **both** `X-GT-USER-ID` and `X-GT-API-KEY`. |
| 403 | Not entitled | The key is valid but not for this tenant/organization/account. |
| 404 | Record not found | The id does not exist **in this tenant**. Ids are plain integers and are tenant-scoped. |
| 500 | Server error | Retry with backoff. Capture the `x-gt-trace-id` response header and quote it to support. |

Two different error bodies exist: the standard `{"errors":[{code,message,fields}]}` envelope and
FastAPI's `{"detail":[{loc,msg,type}]}` validation shape. Handle both. There is **no** `429`
declared and no rate-limit headers, so back off on your own schedule.

## Do not

- Do not retry a failed write blindly — there is no idempotency key.
- Do not assume `429` semantics; the API declares none.
- Do not use the `session` security scheme from a server-side client. It is referenced by 248
  operations but is not defined in the spec; use the API key pair.
