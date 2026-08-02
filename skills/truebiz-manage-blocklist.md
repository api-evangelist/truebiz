---
name: Manage the TrueBiz domain blocklist
description: Evaluate a domain against your blocklist and add or remove blocklisted domains.
api: openapi/truebiz-openapi-original.json
operations:
  - core_api_async_company_blocklist_evaluate_blocked_domains
  - core_api_company_blocklist_create_blocked_domain
  - core_api_company_blocklist_list_blocked_domains
  - core_api_company_blocklist_delete_blocked_domain
---

# Manage the TrueBiz domain blocklist

Use this to enforce your own deny-list of merchant domains during intake.

## Auth
Send your API key in the `X-API-KEY` header. Base URL `https://ae.truebiz.io`.

## Steps

1. **Evaluate a domain.** `core_api_async_company_blocklist_evaluate_blocked_domains` — `POST /api/v1/company/evaluate` — to check whether a domain matches your blocklist before proceeding.

2. **Add a domain.** `core_api_company_blocklist_create_blocked_domain` — `POST /api/v1/company/block`.

3. **List blocked domains.** `core_api_company_blocklist_list_blocked_domains` — `GET /api/v1/company/block` (paginate with `limit`/`offset`).

4. **Remove a domain.** `core_api_company_blocklist_delete_blocked_domain` — `DELETE /api/v1/company/block`.

## Rules
- Adding an already-blocked domain may return `409 Conflict`; treat it as already-present.
- Errors are JSON with a `detail` field; handle `401`, `422`, and `429`. See `errors/truebiz-problem-types.yml`.
