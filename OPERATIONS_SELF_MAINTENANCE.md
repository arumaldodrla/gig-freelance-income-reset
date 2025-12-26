# Operations & Self-Maintenance

**Version: 4.0**

This document outlines the automated operational procedures for the Freelancer Financial Hub. The system is designed to be self-monitoring and self-healing, allowing the team to focus on product development rather than operational firefighting.

## 1. The Automated Weekly Health Check ("Sunday Checkup")

Every Sunday at 02:00 UTC, a Supabase scheduled function (`weekly-health-check`) will execute a series of checks focused on the core value propositions of the product.

| Check Category | Specific Checks | Business Impact & Rationale |
| :--- | :--- | :--- |
| **Income Sync Integrity** | - Ping all supported gig platform APIs (Upwork, Fiverr, etc.) to ensure they are reachable.<br>- Check for users whose `income_sources` have not synced in over 24 hours.<br>- Validate a sample of recent transactions against known good data. | **Protects Core Value.** The entire product is built on accurate income aggregation. A failure here means the product is not delivering on its primary promise. |
| **Tax Calculation Accuracy** | - Run the tax calculation engine on a set of predefined user profiles with known income/expense data.<br>- Compare the output against a set of expected, manually calculated tax liabilities. | **Builds User Trust.** An incorrect tax calculation can have serious financial consequences for the user and would destroy the credibility of the product. |
| **Data Integrity** | - Check for orphaned records (e.g., `transactions` without a valid `user_id`).<br>- Ensure that all `expenses` have a valid category. | **Prevents Data Corruption.** Maintains the health and accuracy of the user's financial data, which is the core asset of the application. |
| **Revenue Path** | - Ping Zoho Billing and Authorize.net APIs.<br>- Simulate a trial signup and an upgrade API call in a sandboxed test account. | **Prevents Revenue Loss.** Ensures our own business's monetization path is always open. |

## 2. Automated Actions & Escalation Logic

The system is empowered to take specific, safe actions automatically.

### Allowed Automated Actions

- **Retry Failed Syncs:** Automatically retry failed income sync jobs with an exponential backoff strategy.
- **Isolate Failing Integrations:** If a user's connection to a specific gig platform fails repeatedly, automatically mark the `income_source` as `NEEDS_REAUTH` and trigger a notification to the user.
- **Data Pruning:** Automatically prune old, inactive trial accounts that do not convert to paid subscriptions.

### Forbidden Automated Actions (Require Human Intervention)

- **Modifying Financial Data:** The system will **never** automatically alter a user's `transactions` or `expenses` records after they have been created.
- **Altering Tax Calculations:** The core tax calculation formulas can only be changed via a new code deployment that has been reviewed and approved.

## 3. Automated Reporting & Alerting

The weekly checkup has a clear, tiered reporting structure:

| Status | Anomaly Level | Action |
| :--- | :--- | :--- |
| **Green** | No issues found. | A single "All Systems Green" message is posted to the internal `#ops-status` Slack channel. |
| **Yellow** | Minor, non-critical issue (e.g., a single platform API was slow to respond). | A detailed report is emailed to the `engineering@` distribution list for review. |
| **Red** | Critical P1/P2 issue (e.g., tax calculations are incorrect, income sync is failing for all users). | 1. An alert is sent to PagerDuty to notify the on-call engineer.<br>2. A high-priority ticket is automatically created in Zoho Desk.<br>3. A critical alert message is posted in the `#ops-status` Slack channel. |
