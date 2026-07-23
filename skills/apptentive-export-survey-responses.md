---
name: Export Apptentive survey responses
description: Enumerate surveys for an app and export their quantitative stats and raw text responses from the Apptentive/Alchemer Digital Data API.
api: openapi/apptentive-openapi-original.yml
operations: [getSurveyStats, getOneSurveyStats, getSurveyResponses, getRawSurveyResponses]
---

# Export Apptentive survey responses

Read-only flow over `https://data.apptentive.com`.

## Auth
`X-API-KEY` header on every request. Rate limit 100 requests / 5 minutes per IP.

## Steps
1. `getSurveyStats` — list survey stats for the app to discover `survey_id`s.
2. `getOneSurveyStats` — quantitative reporting stats for one `survey_id` over an optional date range.
3. `getSurveyResponses` — text responses for a specific `survey_id` + `question_id`; paginate with `page` + `limit` (max 1000).
4. `getRawSurveyResponses` — full raw survey responses; cursor-paginate with `page_size` + `starts_after`, order `asc`/`desc`.

## Rules
- Ids are Mongo ObjectIds; dates are `YYYY-MM-DD`.
- Raw list endpoints use cursor pagination (`starts_after` = last id from the prior page); reporting endpoints use `page`/`limit`. See conventions/apptentive-conventions.yml.
- Errors follow errors/apptentive-problem-types.yml.
