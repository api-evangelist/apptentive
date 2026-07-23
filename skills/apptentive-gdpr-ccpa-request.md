---
name: Submit and track an Apptentive GDPR/CCPA request
description: Trigger a GDPR/CCPA data subject request for an app and poll its status via the Apptentive/Alchemer Digital Data API.
api: openapi/apptentive-openapi-original.yml
operations: [postGDPRRequest, getGDPRRequest]
---

# Submit and track an Apptentive GDPR/CCPA request

The one write flow on the Data API, over `https://data.apptentive.com`.

## Auth
`X-API-KEY` header on every request. Rate limit 100 requests / 5 minutes per IP.

## Steps
1. `postGDPRRequest` — POST to `/metrics/v2/apps/{app_id}/gdpr-ccpa/requests` with the JSON `gdprPayload` body (application/json). On success returns `201` with the created request id.
2. `getGDPRRequest` — poll `/metrics/v2/apps/{app_id}/gdpr-ccpa/requests/{request_id}` for status. `404` means the id is unknown.

## Rules
- This is a state-changing (write) operation; there is no documented idempotency key, so avoid blind retries on `postGDPRRequest` — re-check status with `getGDPRRequest` before re-submitting.
- `app_id` and `request_id` are Mongo ObjectIds.
- Errors follow errors/apptentive-problem-types.yml (400/401/404/500).
