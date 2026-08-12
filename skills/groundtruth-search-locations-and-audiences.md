---
name: groundtruth-search-locations-and-audiences
description: Build GroundTruth targeting — search points of interest, location groups, neighborhoods, drive-to and on-premise locations, and audience segments — before attaching them to an ad group.
api: openapi/groundtruth-ads-manager-openapi.yml
base_url: https://api-public.groundtruth.com
generated: '2026-08-12'
method: generated
source: openapi/groundtruth-ads-manager-openapi.yml (every operationId below verified present in the live spec)
operations:
  - search_location_groups
  - search_location_group_pois
  - search_pois_sources
  - search_pois_sources_count
  - search_pois_sources_preview_pois
  - search_pois_location_filters
  - search_location_filters
  - search_neighborhoods
  - search_drive_to_locations
  - search_on_premise_locations
  - search_audiences
  - get_audience_recommendations
  - search_campaigns
  - search_adgroups
  - search_creatives
  - search_accounts
---

# Build GroundTruth targeting

Location is what GroundTruth sells. The 18 `/search/*` operations are how you resolve real-world
places and audience segments into the ids an ad group accepts. They are all **POST** with a JSON
body — they are searches, not idempotent GETs, despite reading nothing.

## Before you start

Both auth headers, plus `tenant_id`:

```
X-GT-USER-ID: <your user id>
X-GT-API-KEY: <your api key>
```

## Location targeting

Work outside-in: find the source, count it, preview it, then commit it to a location group.

1. `search_pois_sources` (POST `/search/location_groups/sources`) — find the POI sources available
   (brands, categories, chains).
2. `search_pois_sources_count` (POST `/search/location_groups/sources/count`) — how many points of
   interest a candidate source actually yields. **Do this before you build the group**; it is the
   cheapest way to find out that a targeting idea is too thin or too broad.
3. `search_pois_sources_preview_pois` (POST `/search/location_groups/sources/pois`) — look at the
   actual POIs the source resolves to.
4. `search_location_groups` (POST `/search/location_groups`) and `search_location_group_pois`
   (POST `/search/location_groups/pois`) — work with existing groups.
5. `search_location_filters` and `search_pois_location_filters` — the filter layer over a group.
   `download_location_filters` exports an ad group's filters.

Other place primitives:

- `search_neighborhoods` — neighborhood polygons.
- `search_drive_to_locations` — destinations for drive-to campaigns.
- `search_on_premise_locations` — on-premise targeting.

Several location operations take a bounding box: `sw_lat`, `sw_lng`, `ne_lat`, `ne_lng`. Send all
four or none.

## Audience targeting

- `search_audiences` (POST `/search/audiences`) — find segments in the taxonomy.
- `get_audience_recommendations` (POST `/audiences/recommendations`) — ask the platform what to
  target given your inputs, rather than picking blind.

Reference vocabularies for audience and content targeting come from the **unauthenticated**
`/static/*` lookups: `get_iab_categories`, `get_publisher_categories`, `get_account_verticals`,
`get_dmas`, `get_audio_podcast_topics`, `get_audio_streaming_genres`,
`get_streaming_video_content_categories`, `get_streaming_video_premium_ctv_categories`. Cache these;
they change rarely and cost you nothing.

## Finding existing objects

The same `/search/*` family covers the account hierarchy — `search_tenants`, `search_organizations`,
`search_accounts`, `search_campaigns`, `search_adgroups`, `search_creatives`, `search_ad_packages`.
Use these instead of paging `get_campaigns` when you are looking for something by name.

## Then attach it

Feed the resulting ids into `create_adgroup` or `patch_adgroup`. `AdgroupModel` carries
`location_group_ids`, `poi_ids`, `dma_ids`, `category_ids` and `brand_ids` — all plain integers,
all tenant-scoped. Verify inventory with `get_adgroup_avails` before you attach creatives.

## Do not

- Do not invent category, DMA, POI or brand ids. Every one of them comes from a `/static/*` lookup
  or a `/search/*` result. A guessed id will either 404 or silently target the wrong thing.
- Do not treat a POST `/search/*` as safe to retry into a write path — it is a read, but the ad group
  update that follows is not, and there is no idempotency key.
