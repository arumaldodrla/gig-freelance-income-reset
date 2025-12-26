# Observability & Incidents

**Version:** 4.0

This document outlines the observability and incident management strategy for the **Freelancer Financial Hub**. Our goal is to detect and resolve issues before they impact our users, ensuring the reliability and trustworthiness of our financial data.

## 1. The Golden Signals (Mapped to Business KPIs)

We will monitor the four "golden signals" for every service, with a direct line to the business outcomes they support.

| Golden Signal | Key Metrics to Monitor | Business KPI Impacted |
| :--- | :--- | :--- |
| **Latency** | - `api_request_duration_p95`: 95th percentile latency for all API endpoints.<br>- `income_sync_job_duration`: Time taken to complete a user's income sync. | **User Experience.** Slow dashboards and syncs lead to frustration and churn. |
| **Traffic** | - `api_requests_per_second`: Overall load on the system.<br>- `active_users_15min`: Number of active users in a 15-minute window. | **Growth & Adoption.** Tracks user engagement and helps with capacity planning. |
| **Errors** | - `api_error_rate_5xx`: Server-side error rate.<br>- `income_sync_failure_rate`: Percentage of income sync jobs that fail.<br>- `tax_calculation_errors`: Number of times the tax engine fails. | **Trust & Reliability.** High error rates, especially in core financial calculations, destroy user trust. |
| **Saturation** | - `supabase_cpu_utilization`: CPU usage of our database.<br>- `supabase_db_connections`: Number of active database connections. | **Cost & Scalability.** Helps us anticipate scaling needs and manage our infrastructure costs. |

## 2. Dashboards & Alerting

- **Core Business Dashboard:** A high-level dashboard for the product team showing user signups, conversion rates, and active users.
- **Engineering Health Dashboard:** A detailed dashboard for the engineering team showing the golden signals for all services, with a particular focus on the `income_sync_failure_rate` and `tax_calculation_errors`.
- **Alerting:** We will use PagerDuty for alerting, with a clear distinction between high-urgency (P1) and low-urgency (P2) alerts.
    - **P1 Alert (Pages the on-call engineer):** `income_sync_failure_rate` > 10% for 5 minutes; `api_error_rate_5xx` > 5% for 5 minutes.
    - **P2 Alert (Creates a ticket/Slack message):** `api_request_duration_p95` > 2 seconds for 10 minutes.

## 3. Incident Response Playbook

In the event of a P1 incident, the on-call engineer will follow this playbook:

1.  **Acknowledge:** Acknowledge the PagerDuty alert.
2.  **Assess:** Go to the Engineering Health Dashboard to understand the scope of the impact. Is it affecting all users or a subset? Is it related to a recent deployment?
3.  **Communicate:** Post a message in the `#incidents` Slack channel: `P1 Incident: [Brief Description]. I am investigating.`
4.  **Mitigate:** The primary goal is to stop the bleeding. The fastest path to mitigation is usually a **frontend rollback** via Vercel, which takes < 1 minute. If the issue is in the backend, consider disabling a feature via a feature flag.
5.  **Resolve:** Once the incident is mitigated, investigate the root cause.
6.  **Postmortem:** All P1 incidents require a blameless postmortem to be completed within 48 hours.
