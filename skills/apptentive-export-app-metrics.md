---
name: Export Apptentive app metrics
description: Discover an app and pull its core engagement metrics (ratings, reviews, retention, Fan Signals) over a date range from the Apptentive/Alchemer Digital Data API.
api: openapi/apptentive-openapi-original.yml
operations: [getAllApps, getActiveUsersData, getRatings, fetchReviews, getRetention, getNFSData]
---

# Export Apptentive app metrics

Read-only reporting flow over `https://data.apptentive.com`.

## Auth
Send the `X-API-KEY` header (issued from the Alchemer Digital dashboard) on every request. `api_key` query parameter is an optional alternative. Requests are rate limited to 100 per 5 minutes per IP.

## Steps
1. `getAllApps` — list the apps associated with your key; capture the `app_id` (a Mongo ObjectId like `5e22085d5379215be800002b`).
2. `getActiveUsersData` — active users for the app; pass `app_id`, required `start_date`/`end_date` (YYYY-MM-DD), optional `period`.
3. `getRatings` — star ratings over the range (supports `text/csv`).
4. `fetchReviews` — app store reviews; cursor-paginate with `page_size` + `starts_after`.
5. `getRetention` — retention; `period` must be `monthly` or `daily`.
6. `getNFSData` — Net Fan Score over the range.

## Rules
- Dates are `YYYY-MM-DD`; most metrics endpoints require both `start_date` and `end_date`.
- Handle `401` (bad/missing key), `400` (bad params), `500` (retry with backoff) — see errors/apptentive-problem-types.yml.
- Back off to stay under 100 requests / 5 minutes.
