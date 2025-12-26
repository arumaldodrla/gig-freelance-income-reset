# Implementation Backlog

**Version: 4.0**

This document outlines the prioritized implementation backlog for the Freelancer Financial Hub, structured as a 12-week plan to deliver the Minimum Viable Product (MVP). The backlog is organized by the pain points it solves, ensuring a laser focus on delivering value to the user.

## Phase 1: The Foundation - Income & Tax Clarity (Weeks 1-5)

**Goal:** Solve the two most severe pain points: not knowing one's true income and not knowing how much tax is owed. This phase delivers the core "magic moment" of the product.

| Epic (Pain Point) | User Story | Key Tasks |
| :--- | :--- | :--- |
| **Income Visibility** | As a new user, I want to connect my Upwork account and see my YTD income in one place. | - Build the user signup/login flow with Supabase Auth.<br>- Create the `income_sources` and `transactions` tables.<br>- Build the API client and sync logic for the first gig platform (e.g., Upwork).<br>- Design and build the main dashboard UI with a prominent YTD income display. |
| **Tax Uncertainty** | As an active user, I want to see a real-time estimate of my quarterly tax liability based on my income. | - Build the initial Tax Calculation Engine based on the standard self-employment tax formula.<br>- Create the `tax_estimates` table.<br>- Integrate the engine with the backend so that taxes are recalculated after every income sync.<br>- Display the current quarterly tax estimate on the dashboard. |

## Phase 2: Building Trust & Utility (Weeks 6-9)

**Goal:** Build on the foundation by solving the next two major pain points: tracking expenses to lower the tax bill and proving income to unlock financial products.

| Epic (Pain Point) | User Story | Key Tasks |
| :--- | :--- | :--- |
| **Losing Deductions** | As a user, I want to manually enter my business expenses to lower my taxable income. | - Create the `expenses` table.<br>- Build the expense entry form and a list view of all expenses.<br>- Integrate expense data into the Tax Calculation Engine.<br>- Add an expense breakdown visualization to the dashboard. |
| **Losing Deductions (AI)** | As a user, I want to scan a receipt with my phone to automatically create an expense. | - Integrate with Gemini Vision for OCR.<br>- Build the mobile-friendly receipt upload UI.<br>- Create the backend logic to handle the image upload and pre-fill the expense form with the OCR data. |
| **Proving Income** | As a user, I want to download a professional PDF letter that summarizes my income for the last 12 months. | - Build the PDF generation service/function.<br>- Create the backend endpoint that aggregates user income data and calls the PDF service.<br>- Add a "Download Income Letter" button to the dashboard. |

## Phase 3: Polish & Proactive Support (Weeks 10-12)

**Goal:** Round out the MVP with features that make the product feel proactive, supportive, and ready for public launch.

| Epic (Pain Point) | User Story | Key Tasks |
| :--- | :--- | :--- |
| **Tax Uncertainty** | As a user, I want to be reminded before my quarterly tax payments are due. | - Integrate with an email service (Resend/Zoho Mail).<br>- Create a scheduled job that checks for upcoming tax deadlines.<br>- Build and send the tax reminder email, including the estimated amount due. |
| **General Support** | As a user with a question, I want to get an instant answer from an AI assistant. | - Integrate with an LLM for the in-app chat support.<br>- Build the AI assistant chat interface.<br>- Implement the escalation path to create a Zoho Desk ticket. |
| **Operational Readiness** | As a developer, I want to have confidence in the stability of the platform. | - Implement the automated weekly health check (`Sunday Checkup`).<br>- Set up dashboards and alerting for key metrics (sync failures, tax calculation errors).<br>- Conduct a final round of E2E and security testing. |
