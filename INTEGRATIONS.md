# Integrations

**Version:** 1.1

This document details the integration strategy for all external services, which is a cornerstone of our ability to deliver a feature-rich product with a lean team and low operating costs.

## 1. Zoho Ecosystem: The Business Operations Backbone

Our integration with the Zoho suite is a core strategic advantage, estimated to **reduce development and operational overhead by 60%**. By using Zoho as the system-of-record for all business operations, we avoid building complex, non-differentiating systems for billing, CRM, and accounting.

**CRITICAL PRINCIPLE:** The user-facing UI for all functions **MUST** reside within our platform. Zoho is a backend system accessed via API only. **No user will ever be redirected to a Zoho portal for any standard workflow.**

### 1.1. Zoho CRM

- **Purpose:** The single source of truth for the customer lifecycle.
- **Data Exchanged:**
    - **Outbound:** On user signup, create a new Contact in Zoho CRM.
    - **Inbound/Outbound:** Sync subscription status (`plan`, `status`) as Tags (e.g., `Trial`, `Pro`, `Cancelled`) for segmentation.
- **Key Workflow:** When a user upgrades from `Trial` to `Starter`, the backend must update the contact's tag in Zoho CRM from `Trial` to `Starter`.

### 1.2. Zoho Billing

- **Purpose:** To manage all aspects of the subscription lifecycle, from trials to renewals and cancellations.
- **Data Exchanged:**
    - **Outbound:** Create a new subscription when a user starts a trial. Upgrade/downgrade subscriptions when a user changes plans. Process payments via the integrated Authorize.net gateway.
    - **Inbound:** Receive webhook notifications for billing events (e.g., `subscription_activated`, `payment_success`, `payment_failure`, `subscription_cancelled`).
- **Key Workflow:** When a user's payment fails, a webhook from Zoho Billing triggers a backend process to update the user's subscription status to `PAST_DUE` and initiate dunning emails.

### 1.3. Zoho Books

- **Purpose:** To maintain accurate financial records for the business itself (not for the end-user's accounting).
- **Data Exchanged:**
    - **Outbound:** When a subscription payment is successfully processed by Zoho Billing, an invoice and payment record are automatically created in Zoho Books.
- **Key Workflow:** This integration is largely automated by the Zoho ecosystem itself. Our responsibility is to ensure that the initial setup in Zoho Billing correctly links to Zoho Books.

### 1.4. Zoho Desk

- **Purpose:** The escalation point for the AI-first support model.
- **Data Exchanged:**
    - **Outbound:** When the in-app AI assistant cannot resolve an issue or the user explicitly requests human help, the backend will create a new ticket in Zoho Desk. The ticket **must** include the user's ID, email, subscription plan, and a full transcript of the AI conversation for context.
- **Key Workflow:** The AI assistant determines that escalation is needed, calls a backend function `createSupportTicket`, which then calls the Zoho Desk API.

## 2. Authorize.net: Payment Processing

- **Purpose:** To securely process credit card payments.
- **Primary Integration Path:** Via Zoho Billing's pre-built integration. This is the default and preferred method as it dramatically reduces our PCI-DSS compliance scope to SAQ-A.
- **Data Flow:** Our frontend will use a hosted payment form (an iframe provided by Zoho/Authorize.net) to collect card details. The card data never touches our servers. A secure payment token is sent to our backend, which we then pass to Zoho Billing to associate with the subscription.

## 3. AI Services: The Intelligence Layer

- **Purpose:** To power the core automated features of the product.
- **Services:**
    - **Gemini Vision (OCR):** Used for the receipt scanning feature. The backend will send an image of a receipt to the Gemini Vision API and receive structured data (vendor, date, amount) in return.
    - **Large Language Models (LLM):** Used for the AI support assistant and for generating financial insights. The backend will send prompts (e.g., a user's support query, a summary of a user's income data) to the LLM API and receive a natural language response.
- **Cost Management:** API usage for AI services is a direct cost. The architecture must include monitoring and rate-limiting to manage these costs, which are projected to be $360-$1,200 in Year 1.

## 4. Data Consistency & Error Handling

- **Idempotency:** All API calls that create or modify data in external systems (especially Zoho Billing and Books) **must** be idempotent. Use unique transaction IDs or other mechanisms to prevent duplicate records.
- **Retry Logic:** Implement an exponential backoff retry strategy for all critical API calls to external systems to handle transient network failures.
- **Circuit Breaker:** For high-volume integrations like income source syncing, implement a circuit breaker pattern to prevent cascading failures if a third-party API is down.
- **Dead-Letter Queue:** For critical webhook events from Zoho, use a dead-letter queue to store any events that fail to be processed after multiple retries. This allows for manual inspection and reprocessing, ensuring no revenue-critical events are lost.
