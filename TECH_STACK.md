# Technology Stack

**Version:** 4.0

This document provides a definitive reference for the technology stack used to build and operate the **Freelancer Financial Hub**.

## 1. Frontend

| Technology | Version | Purpose & Rationale |
| :--- | :--- | :--- |
| **React** | 18.x | The core UI library for building the user interface. |
| **Vite** | 5.x | The frontend build tool, chosen for its speed and developer experience. |
| **Refine** | 4.x | A React-based framework that provides out-of-the-box components for authentication, data grids, and forms, accelerating development. |
| **Tailwind CSS** | 3.x | A utility-first CSS framework for rapidly building custom designs. |
| **Recharts** | 2.x | A composable charting library for building the data visualizations on the dashboard. |
| **i18next** | 23.x | The internationalization framework for managing translations (English, Spanish, and future languages). |

## 2. Backend

| Technology | Version | Purpose & Rationale |
| :--- | :--- | :--- |
| **Supabase** | Latest | The core of our backend, providing a PostgreSQL database, authentication, serverless functions, and real-time capabilities in a single, managed platform. |
| **PostgreSQL** | 15.x | The underlying relational database, chosen for its reliability and powerful features like Row-Level Security. |
| **Node.js** | 20.x | The runtime for our Supabase Edge Functions. |

## 3. Integrations & Services

| Service | Purpose & Rationale |
| :--- | :--- |
| **Zoho** | The backbone of our business operations (CRM, Billing, Books, Desk). |
| **Authorize.net** | The payment processor for our subscription billing, integrated via Zoho Billing. |
| **Google Cloud AI** | Provides the AI services for receipt scanning (Gemini Vision) and future income forecasting. |
| **Resend / Zoho Mail** | Used for sending transactional emails (e.g., tax reminders, weekly summaries). |

## 4. Development & Operations

| Tool | Purpose & Rationale |
| :--- | :--- |
| **GitHub** | The source code repository and CI/CD platform (via GitHub Actions). |
| **Vercel** | The hosting platform for our frontend, chosen for its seamless integration with Vite and its global CDN. |
| **PagerDuty** | The alerting and on-call management tool for incident response. |

## 5. Environment Variables

A comprehensive list of all required environment variables will be maintained in the Vercel and Supabase project settings. **These will never be committed to the repository.**

- `SUPABASE_URL`
- `SUPABASE_ANON_KEY`
- `ZOHO_CLIENT_ID`
- `ZOHO_CLIENT_SECRET`
- `GOOGLE_AI_API_KEY`
- `RESEND_API_KEY`
