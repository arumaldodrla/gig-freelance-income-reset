# Privacy, GDPR, and DPIA

**Version:** 4.0

This document outlines the privacy framework for the **Freelancer Financial Hub**. Our approach to privacy is a core component of our brand identity, designed to build deep, lasting trust with our users.

## 1. Privacy as a Product Feature

We treat privacy not as a legal burden, but as a core product feature. Our target users are rightly concerned about how their financial data is used. By being transparent, providing clear controls, and adhering to the highest standards, we turn privacy into a competitive advantage.

## 2. Lawful Basis for Processing

Our data processing activities are grounded in the following lawful bases under GDPR:

- **Performance of a Contract:** The majority of our data processing is necessary to deliver the service the user has signed up for. This includes processing income/expense data to provide tax estimates and aggregating data for the dashboard.
- **Consent:** For activities not covered under contract, we will obtain explicit, informed consent. This includes:
    - **Marketing Communications:** Users must actively opt-in to receive marketing emails.
    - **AI Feature Usage:** Users will be shown a clear, one-time consent screen before using features that send their data to third-party AI subprocessors (e.g., receipt scanning).
- **Legitimate Interest:** We rely on legitimate interest for activities like security logging and fraud prevention.

## 3. Data Inventory & Retention Policy

| Data Category | Purpose | Lawful Basis | Retention Period |
| :--- | :--- | :--- | :--- |
| **User Profile Data** | Account creation and management. | Contract | Duration of account + 1 year for recovery. |
| **Financial Data (Income, Expenses)** | To provide the core service features. | Contract | Duration of account. Anonymized for trend analysis. |
| **Subscription & Billing Data** | To manage the user's subscription plan. | Contract | Duration of account + 7 years for financial audits. |
| **AI-Processed Data (Receipts)** | To power the OCR expense feature. | Consent | Data is deleted from the subprocessor immediately after processing. |
| **Usage Analytics (Anonymized)** | To improve the product and user experience. | Legitimate Interest | Indefinite (as it contains no PII). |
| **Audit Log Data** | Security monitoring and incident response. | Legitimate Interest | 2 years. |

## 4. Data Protection Impact Assessment (DPIA) Summary

A DPIA has been conducted for the core processing activities.

- **Processing Activity:** Automated aggregation of financial data from multiple platforms and AI-powered analysis for tax estimation and insights.
- **Identified Risks:**
    1.  **Information Disclosure:** A data breach could expose highly sensitive financial information.
    2.  **Inaccurate Profiling:** Incorrect AI analysis could lead to inaccurate tax estimates, causing financial harm.
    3.  **Data Creep:** Using data for purposes other than what was consented to.
- **Mitigation Measures:**
    1.  **Risk:** Information Disclosure -> **Mitigation:** End-to-end encryption, strict RLS, data minimization, and regular penetration testing.
    2.  **Risk:** Inaccurate Profiling -> **Mitigation:** Clearly label all AI-generated figures as **estimates** and not financial advice. Provide users with the ability to manually override and correct data.
    3.  **Risk:** Data Creep -> **Mitigation:** Strict purpose limitation enforced via code and policy. All new data uses require a new DPIA and, if necessary, fresh user consent.

## 5. Data Subject Rights (DSAR) Automation

To align with our AI-first operational model, Data Subject Access Requests (DSAR) will be automated and self-service where possible.

- **Right of Access & Portability:** A "Download My Data" button in the user's account settings will generate a JSON export of all their data.
- **Right to Erasure ("Right to be Forgotten"):** A "Delete My Account" button will trigger an automated workflow that:
    1.  Immediately deactivates the user's account.
    2.  Initiates a 30-day grace period for recovery.
    3.  After 30 days, permanently deletes all personal data from our primary database and sends erasure requests to all relevant subprocessors (e.g., Zoho).

## 6. Subprocessors

All subprocessors are vetted for their security and privacy posture. A list of subprocessors will be maintained in our Privacy Policy.

| Subprocessor | Purpose | Data Center Region(s) |
| :--- | :--- | :--- |
| **Supabase** | Database, Auth, Storage | US (Primary) |
| **Vercel** | Frontend Hosting | Global CDN |
| **Zoho** | CRM, Billing, Books, Desk | US (Primary) |
| **Google Cloud (AI APIs)** | OCR, LLM | US (Primary) |
| **Resend / Zoho Mail** | Transactional Emails | US (Primary) |
