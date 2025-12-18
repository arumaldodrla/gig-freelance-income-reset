# Operations & Self-Maintenance

**Version:** 1.1

This document outlines the operational philosophy and automated self-maintenance procedures for the Gig/Freelance Income Reset application. Our core business strategy relies on a lean team and low operational overhead, making automation a critical component of our success.

## 1. Operational Philosophy: AI-First, Human as Exception

Our operational model is built on the same principle as our customer support model: **AI-first, with human intervention as the exception.** The system is designed to be largely self-healing and self-monitoring, allowing our lean 3-person engineering team to focus on building value, not fighting fires. This is how we maintain our exceptional LTV:CAC ratio and achieve profitability with minimal capital.

## 2. The Automated Weekly Health Check ("Sunday Checkup")

Every Sunday at 02:00 UTC, a Supabase scheduled function (`weekly-health-check`) will execute a comprehensive series of checks. This automated process is our first line of defense against issues that could impact revenue, user trust, or business performance.

| Check Category | Specific Checks | Business Impact & Rationale |
| :--- | :--- | :--- |
| **Infrastructure & Revenue Path** | - Ping critical API endpoints: Zoho Billing, Authorize.net, AI services.<br>- Simulate a trial signup and an upgrade API call in a sandboxed test account. | **Prevents Revenue Loss.** Ensures our core monetization path is always open. A failure here is a P1 incident. |
| **Data Integrity & Consistency** | - Check for orphaned records (e.g., `subscriptions` without a valid `user`).<br>- Validate that all `users` have a corresponding `zoho_crm_contact_id`.<br>- Check for data mismatches between our DB and Zoho Billing. | **Prevents Billing Errors & Protects User Trust.** Ensures that what the user sees in the app matches their actual subscription, preventing customer support nightmares. |
| **Business KPI Monitoring** | - Compare key metrics (trial conversion rate, churn rate) against the financial model's targets.<br>- Monitor the growth rate of key tables (`users`, `transactions`). | **Provides an Early Warning System.** Allows us to detect deviations from our business plan early, before they become major problems. |
| **Security & Anomaly Detection** | - Scan for spikes in failed login attempts.<br>- Monitor for unusual API usage patterns (e.g., a single user syncing data excessively).<br>- Check for users with `gdpr_erasure_requested` flags that have not been processed. | **Proactive Threat Detection.** Helps us identify and respond to potential security threats before they escalate. |

## 3. Automated Actions & Escalation Logic

The system is empowered to take specific, safe, and idempotent actions automatically. Anything outside this scope requires human approval.

### Allowed Automated Actions

- **Retry Failed Jobs:** Automatically retry failed income syncs or webhook processing with an exponential backoff strategy.
- **Isolate Failing Integrations:** If a user's income source connection fails repeatedly, automatically mark the source as `NEEDS_REAUTH` and trigger a notification to the user.
- **Data Cleanup:** Automatically prune expired, non-converted trial accounts and associated data after a grace period.

### Forbidden Automated Actions (Require Human Intervention)

- **Modifying Financial Records:** The system will **never** automatically alter records in Zoho Books or issue refunds.
- **Altering Subscription Data:** The system will **never** change a user's subscription plan or pricing without a direct, user-initiated action.
- **Running Un-audited Code:** The system will **never** deploy new code or run database migrations automatically.

## 4. Automated Reporting & Alerting

The weekly checkup has a clear, tiered reporting structure:

| Status | Anomaly Level | Action |
| :--- | :--- | :--- |
| **Green** | No issues found. | A single "All Systems Green" message is posted to the internal `#ops-status` Slack channel. |
| **Yellow** | Minor, non-critical issue (e.g., KPI deviation, single API timeout). | A detailed report is emailed to the `engineering@` distribution list for review during business hours. |
| **Red** | Critical P1/P2 issue (e.g., revenue path failure, major data inconsistency). | 1.  An alert is sent to PagerDuty to notify the on-call engineer.<br>2.  A high-priority ticket is automatically created in Zoho Desk and assigned to the "Engineering" team.<br>3.  A critical alert message is posted in the `#ops-status` Slack channel. |
