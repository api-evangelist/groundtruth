---
name: groundtruth-pull-campaign-performance
description: Pull GroundTruth campaign, ad group and creative performance — totals, timeseries, day-of-week and time-of-day, product insights, visitation and web engagement — from the Ads Manager reporting surface.
api: openapi/groundtruth-ads-manager-openapi.yml
base_url: https://api-public.groundtruth.com
generated: '2026-08-12'
method: generated
source: openapi/groundtruth-ads-manager-openapi.yml (every operationId below verified present in the live spec)
operations:
  - get_totals_campaign
  - get_totals_adgroup
  - get_totals_creative
  - get_totals_account
  - get_totals_organization
  - get_timeseries_dow_campaign
  - get_timeseries_tod_imp_campaign
  - get_dow_metrics_campaign
  - get_tod_weekday_campaign
  - get_performance_distribution_campaign
  - get_supply_system_distribution_campaign
  - get_product_insights_campaign
  - get_product_insights_campaign_targeting
  - get_product_insights_campaign_audience
  - get_visitation_lag_window_campaign
  - get_web_engagement_campaign
  - get_campaign_spend
---

# Pull GroundTruth campaign performance

160 of the 259 Ads Manager operations are reporting reads. They do not create anything — they
project metrics over the campaign hierarchy. This skill is how to navigate them without guessing.

## Before you start

Send both auth headers on every call:

```
X-GT-USER-ID: <your user id>
X-GT-API-KEY: <your api key>
```

`tenant_id` is a required query parameter. `start_date` and `end_date` are required on 138 of the
reporting operations.

## The shape of the reporting surface

Reporting is organised on two axes. Pick one of each.

**Grain** — every family exists at several levels, and the operationId says which:
`..._campaign` / `..._adgroup` / `..._creative` / `..._account` / `..._organization`.
Several campaign-level operations take `all_adgroups=true` (or `all_creatives=true`) to break the
result out one level down in a single request — prefer that over fanning out N calls.

**Family** — what you are measuring:

| You want | Call |
|---|---|
| Headline numbers for the flight | `get_totals_campaign`, `get_totals_adgroup`, `get_totals_creative` |
| Roll-up above the campaign | `get_totals_account`, `get_totals_organization` |
| A daily/day-of-week series | `get_timeseries_dow_campaign`, `get_timeseries_dow_campaign_adgroups` |
| Hour-of-day shape | `get_timeseries_tod_imp_campaign`, `get_tod_weekday_campaign`, `get_tod_weekend_campaign` |
| Both axes at once | `get_timeseries_dow_tod_campaign` |
| Aggregate DOW / TOD metrics (not a series) | `get_dow_metrics_campaign`, `get_tod_metrics_adgroup` |
| Where impressions ran (device / OS / publisher type) | `get_supply_system_distribution_campaign` with `key=1` device type, `key=2` operating system, `key=3` publisher type |
| Performance split by product | `get_performance_distribution_campaign` with `key=1` product |
| Which tactics and audiences worked | `get_product_insights_campaign_targeting`, `get_product_insights_campaign_audience` (top N, default 5) |
| Store-visit lag | `get_visitation_lag_window_campaign`, `get_visitation_svl_campaign` |
| On-site behaviour after the click | `get_web_engagement_campaign`, `get_web_engagement_totals_campaign` |
| Spend | `get_campaign_spend`, `get_adgroup_spend` |
| Foot-traffic attribution for a direct-mail order | `get_foot_traffic_attribution_v2_order_summary`, `get_foot_traffic_attribution_v2_detailed` |

Note the `_v2` variants (`get_totals_account_v2`, `get_audience_affinity_campaign_v2`). Both
generations are live; the v2 shape is the newer one. There is no published deprecation policy
telling you when v1 goes away — see `lifecycle/groundtruth-lifecycle.yml`.

## Trim the payload

16 reporting operations accept a `fields` query parameter: a comma-separated allow-list of metric
columns. If you omit it you get **every** column. On the per-adgroup and per-creative time-of-day
timeseries operations at least one field is required. Each operation enumerates its own legal values
in the parameter description in the spec — read them there; they differ per operation.

## Reading the response

Collection responses use the page-number envelope:

```
{ total_count, limit, page_num, total_pages, has_next_page, has_prev_page, items: [...] }
```

`limit` defaults to 10 and caps at 100. `page_num` is 1-indexed. Loop while `has_next_page` is true.
There is no cursor and no `Link` header.

## The other reporting API

For pulling data **out** into an agency or partner reporting stack, GroundTruth also publishes a
separate read-only Reporting API at `https://reporting.groundtruth.com` — 59 GET operations under
`/demand/v1/...`, same `X-GT-USER-ID` / `X-GT-API-KEY` auth. It is a smaller, flatter surface
(account/campaign/adgroup/creative totals and dailies, product, location and audience breakdowns).
See `openapi/groundtruth-reporting-openapi.yml`.

Two constraints on that API specifically:
- The provider states **all its endpoints currently require HTTP/1.1** (`curl --http1.1`).
- Its specification declares **only `200` responses** — no error responses are described at all. An
  unauthenticated call returns `401` with body `{"message":""}` and header
  `x-amzn-ErrorType: UnauthorizedException`, none of which is in the contract.

## Do not

- Do not fan out per-adgroup calls when `all_adgroups=true` gives you the breakout in one request.
- Do not expect a `429` or a `Retry-After` — the API declares neither. Pace long backfills yourself.
- Do not assume the reporting numbers are stable intraday; there is no published data-freshness SLA.
