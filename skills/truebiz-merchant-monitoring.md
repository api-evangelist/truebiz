---
name: Monitor a merchant domain with TrueBiz
description: Start continuous monitoring of a domain and review the alerts TrueBiz raises over time.
api: openapi/truebiz-openapi-original.json
operations:
  - monitoring_api_router_domains_routes_start_start_monitoring_domain
  - monitoring_api_router_domains_routes_list_domains_list_monitored_domains
  - monitoring_api_router_alerts_routes_list_alerts_list_monitoring_alerts
  - monitoring_api_router_alerts_routes_get_alert_get_monitoring_alert
---

# Monitor a merchant domain with TrueBiz

Use this to keep watching a merchant after onboarding and react to new risk signals.

## Auth
Send your API key in the `X-API-KEY` header. Base URL `https://ae.truebiz.io`.

## Steps

1. **Start monitoring.** `monitoring_api_router_domains_routes_start_start_monitoring_domain` — `POST /api/v1/monitoring/start` with the `domain` (and optional `external_ref_id`) you want watched.

2. **Confirm it is enrolled.** `monitoring_api_router_domains_routes_list_domains_list_monitored_domains` — `GET /api/v1/monitoring/domains` (filter by `domain`, `package_type`, `external_ref_id`; paginate with `limit`/`offset`).

3. **Poll for alerts.** `monitoring_api_router_alerts_routes_list_alerts_list_monitoring_alerts` — `GET /api/v1/monitoring/alerts` filtered by `created_after`/`created_before`, `domain`, `flagged_categories`, or `external_ref_id`.

4. **Inspect a specific alert.** `monitoring_api_router_alerts_routes_get_alert_get_monitoring_alert` — `GET /api/v1/monitoring/alerts/{alert_id}`.

## Rules
- There is no webhook delivery — poll the alerts endpoint on your own cadence.
- Respect `429` responses with backoff. Errors carry a `detail` field (`errors/truebiz-problem-types.yml`).
