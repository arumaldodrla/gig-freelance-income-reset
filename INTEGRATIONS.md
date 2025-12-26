# Integrations

**Version: 4.0**

This document details the integration strategy for the Freelancer Financial Hub. The integrations are chosen to directly support the solutions for the five core freelancer pain points.

## 1. Gig Platform APIs: The Income Aggregation Engine

- **Purpose:** This is the most critical integration, directly solving the #1 pain point of income visibility. The goal is to connect to as many freelance and gig economy platforms as possible.
- **Integration Path:**
    - **Primary:** Direct API integrations with platforms that offer them (e.g., Upwork, Fiverr). We will build robust, resilient API clients for each.
    - **Secondary:** For platforms without APIs, we will provide a manual CSV import and a clear manual entry form.
- **Data Flow:** A Supabase cron job will trigger the Platform Sync Engine every 6 hours. The engine will call the respective API for each connected `income_source`, fetch new transactions, and save them to our `transactions` table.
- **Key Challenge:** Each platform has a different API, authentication method, and rate limits. The integration clients must be built with this heterogeneity in mind.

## 2. AI Services: The Intelligence Layer

- **Purpose:** To power the advanced features that differentiate our product, such as receipt scanning and income forecasting.
- **Services:**
    - **Gemini Vision (OCR):** Solves the expense tracking pain point by allowing users to simply photograph a receipt. The backend sends the image to the Gemini Vision API and receives structured data (vendor, date, amount) to pre-fill an expense entry.
    - **Large Language Models (LLM):**
        - **Income Forecasting (Post-MVP):** An LLM will be used to analyze a user's historical income data (as a time-series) and provide a predictive forecast.
        - **AI Support:** An LLM will power the in-app support assistant.

## 3. Zoho Ecosystem: The Business Operations Backbone

- **Purpose:** To manage our business operations (subscriptions, CRM, accounting) so we can focus on building the core product.
- **Integrations:**
    - **Zoho Billing & Authorize.net:** To manage the application's own subscription lifecycle. The user's payment for our service is handled here.
    - **Zoho CRM:** To manage our customer relationships and segment users based on their subscription plan.
    - **Zoho Books:** To sync our application's revenue and user data for our own internal accounting. **Crucially, we also use the Zoho Books API to add credibility to the user's generated income verification letters.** When a user requests a letter, we can optionally sync their verified income data from our platform to their own connected Zoho Books account, and then pull it back for the letter, adding a layer of third-party verification.
    - **Zoho Desk:** As the escalation point for the AI support assistant.

## 4. PDF Generation Service

- **Purpose:** To solve the "prove my income" pain point by generating professional income verification letters.
- **Integration Path:** The backend will call a dedicated PDF generation service (or a library like `fpdf2` running in a Supabase Function). It will send the user's aggregated income data, and the service will return a formatted, professional-looking PDF.

## 5. Email Service (Resend / Zoho Mail)

- **Purpose:** To provide proactive communication and reminders, a key part of the tax and budgeting solutions.
- **Services:**
    - **Tax Reminders:** Send automated emails 30 days before quarterly tax deadlines with the estimated amount due.
    - **Weekly Summaries:** Send a weekly email digest of income earned, progress toward goals, and the updated tax estimate.
