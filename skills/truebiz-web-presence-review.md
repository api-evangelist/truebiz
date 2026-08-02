---
name: Run a TrueBiz Web Presence Review
description: Assess a merchant's legitimacy and risk from a domain plus submitted business details, synchronously or via submit-and-poll.
api: openapi/truebiz-openapi-original.json
operations:
  - core_api_company_review_company
  - core_api_async_company_async_review_company
  - core_api_company_lookup_get_web_presence_review_history
---

# Run a TrueBiz Web Presence Review

Use this to score a merchant's web presence for underwriting or due diligence.

## Auth
Send your API key in the `X-API-KEY` header on every request. Base URL `https://ae.truebiz.io`.

## Steps

1. **Choose sync vs async.**
   - Fast, single lookup: `core_api_company_review_company` — `POST /api/v1/company/search`.
   - Higher volume / long-running: `core_api_async_company_async_review_company` — `POST /api/v1/company/async/search`, which returns a `request_id`.

2. **Send the business details.** Provide at minimum `domain`, plus any of `submitted_business_name`, `submitted_description`, `address_line_1`, `city`, `state_province`, `postal_code`, `country`, `submitted_email`, `submitted_phone`, `submitted_full_name`. Pass `external_tracking_ref` to correlate the result with your own record.

3. **Poll for async results.** For an async submission, poll `core_api_company_lookup_get_web_presence_review_history` — `GET /api/v1/history/company/{request_id}` — until the review is complete.

4. **Read the result.** The response is a `Company` aggregate with `risk_analysis` (including `fraud_risk`), `website_content`, `customer_reviews`, `domain` profile, and connected `people`/`connected_entities`. Use the risk decision/recommendation to drive your underwriting workflow.

## Rules
- Errors are JSON with a `detail` field. Handle `401` (bad `X-API-KEY`), `422` (validation), and `429` (rate limit — back off and retry). See `errors/truebiz-problem-types.yml`.
- Async is submit-and-poll; there is no webhook callback.
