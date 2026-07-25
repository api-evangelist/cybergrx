---
name: Export ecosystem residual risk in bulk
description: Pull an entire third-party ecosystem (residual risk, findings, control scores) from the CyberGRX / ProcessUnity GRX Bulk API in a single request.
api: https://api.cybergrx.com/bulk-v1/swagger/
method: generated
source: https://github.com/CyberGRX/api-examples
operations:
  - "GET /bulk-v1/third-parties"
  - "GET /v2/portfolio/third-parties/{company_id}/risk-profile"
---

# Export ecosystem residual risk in bulk

Operating instructions for retrieving your full vendor ecosystem and its risk
data for reporting / GRC integration.

## Prerequisites
- An account API token in the `Authorization` header.
- Base URL: `https://api.cybergrx.com`.

## Steps
1. **Pull the whole ecosystem in one call.**
   `GET /bulk-v1/third-parties` with header `Authorization: <token>`. Optionally
   scope to reports on/after a date with `?report_date=<YYYY-MM-DD>`. The Bulk
   API returns every third party with its data in a single response — no
   client-side pagination needed.
2. **Read residual risk per third party.** For each third party, the payload
   carries `residual_risk` with:
   - `residual_risk.date` — report date
   - `residual_risk.tier` — assessment tier
   - `residual_risk.scores[]` — per-control scores
   - `residual_risk.findings[]` — gaps / findings
   - `residual_risk.residual_risk_outcomes[]` — outcome rows
   Only **authorized** reports carry the latest residual risk and control scores.
3. **(Optional) Fetch a single vendor's inherent-risk profile** for a worst-case
   view: `GET /v2/portfolio/third-parties/{company_id}/risk-profile`.

## Rules
- Auth: API token in the `Authorization` header.
- The Bulk API is read-optimized (single request); the paginated v1 API is the
  alternative for incremental reads.
- Entity shapes: `data-model/cybergrx-data-model.yml`.
