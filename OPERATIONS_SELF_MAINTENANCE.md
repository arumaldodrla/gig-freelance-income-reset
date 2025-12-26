# Operations & Self-Maintenance

**Version: 4.0**

This document outlines the automated operational procedures for the Freelancer Financial Hub. The system is designed to be self-monitoring and self-healing, allowing the team to focus on product development rather than operational firefighting.

## 1. The AI-Driven Weekly Self-Revision ("Sunday Checkup")

Every Sunday at 02:00 UTC, a master AI agent will initiate a comprehensive self-revision process. This goes beyond a simple health check; it is an active process of self-improvement.

### 1.1. Phase 1: Automated Health Check & Analysis

The system first runs a series of automated checks, identical to a traditional health check, focusing on the core value propositions.


Every Sunday at 02:00 UTC, a Supabase scheduled function (`weekly-health-check`) will execute a series of checks focused on the core value propositions of the product.

| Check Category | Specific Checks | Business Impact & Rationale |
| :--- | :--- | :--- |
| **Income Sync Integrity** | - Ping all supported gig platform APIs (Upwork, Fiverr, etc.) to ensure they are reachable.<br>- Check for users whose `income_sources` have not synced in over 24 hours.<br>- Validate a sample of recent transactions against known good data. | **Protects Core Value.** The entire product is built on accurate income aggregation. A failure here means the product is not delivering on its primary promise. |
| **Tax Calculation Accuracy** | - Run the tax calculation engine on a set of predefined user profiles with known income/expense data.<br>- Compare the output against a set of expected, manually calculated tax liabilities. | **Builds User Trust.** An incorrect tax calculation can have serious financial consequences for the user and would destroy the credibility of the product. |
| **Data Integrity** | - Check for orphaned records (e.g., `transactions` without a valid `user_id`).<br>- Ensure that all `expenses` have a valid category. | **Prevents Data Corruption.** Maintains the health and accuracy of the user's financial data, which is the core asset of the application. |
| **Revenue Path** | - Ping Zoho Billing and Authorize.net APIs.<br>- Simulate a trial signup and an upgrade API call in a sandboxed test account. | **Prevents Revenue Loss.** Ensures our own business's monetization path is always open. |

### 1.2. Phase 2: AI-Powered Code Revision

Based on the analysis from Phase 1, a specialized AI agent will:

- **Identify Minor Issues:** Look for small bugs, performance bottlenecks, or opportunities for code cleanup (e.g., refactoring a function, adding a missing test case).
- **Generate Code Patches:** Automatically generate the code changes to address these minor issues.
- **Create a Pull Request:** Create a PR against the `develop` branch with the proposed changes, titled `chore: AI weekly self-improvement`.

### 1.3. Phase 3: Automated Deployment & Human Approval

- **Minor Updates:** If the PR only contains minor, low-risk changes (e.g., documentation, test improvements, minor bug fixes), it will be **automatically merged and deployed** to the `dev` environment.
- **Major Updates:** If the AI agent determines that a more significant change is needed (e.g., a change to the database schema, a new feature), it will create the PR but will **not** merge it. Instead, it will add a `human-review-required` label and create a task in the project management system for a human engineer to review and approve.

## 2. Escalation Logic

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
