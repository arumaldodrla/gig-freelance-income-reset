# Security & Compliance

**Version: 4.0**

This document outlines the security and compliance strategy for the Freelancer Financial Hub. Handling a user's complete income and expense history is a profound responsibility; therefore, security is our single most important product feature.

## 1. Security as a Core Tenet

Our entire brand is built on trust. A security breach would not just be a technical failure; it would be an existential threat to the business. Our security posture must be robust, transparent, and consistently communicated to the user.

## 2. Compliance Strategy

Our compliance roadmap is designed to build trust and enable market access.

| Framework | Status | Rationale & Key Controls |
| :--- | :--- | :--- |
| **GDPR / CCPA** | **Required at Launch** | As we are handling sensitive financial PII, strict adherence to data subject rights (access, erasure) is mandatory. All data processing will be done with explicit user consent. |
| **PCI-DSS** | **Required at Launch** | Our PCI scope is minimized to **SAQ-A** because we use Zoho Billing and Authorize.net's hosted forms for our own subscription payments. We **never** touch user credit card data. |
| **SOC 2 (Type II)** | **Target: Year 2** | Achieving SOC 2 compliance is a key goal to build enterprise-level trust and will be a major marketing point. Our controls are designed with SOC 2 criteria (Security, Availability, Confidentiality) in mind from day one. |

## 3. Threat Model (STRIDE)

This STRIDE model is updated to reflect the new focus on aggregated financial data.

| Threat Category | Scenario | Mitigation |
| :--- | :--- | :--- |
| **Spoofing** | An attacker impersonates a user to gain access to their complete financial history and tax information. | - **Mitigation:** Enforce Multi-Factor Authentication (MFA). Use Supabase's secure authentication mechanisms. |
| **Tampering** | An attacker intercepts and modifies a user's income or expense data, leading to incorrect tax calculations and financial decisions. | - **Mitigation:** Enforce TLS 1.2+ for all data in transit. Use checksums and validation on all data synced from external platforms. |
| **Repudiation** | A user denies having manually entered a large expense to fraudulently lower their tax bill. | - **Mitigation:** Maintain a detailed, immutable `audit_log` for every transaction, including manual entries, with timestamps and IP addresses. |
| **Information Disclosure** | **(The Highest Risk)** A vulnerability exposes the financial data of all users. | - **Mitigation:** Strict Row-Level Security (RLS) in the database is the primary control, ensuring users can **only** access their own data. All sensitive data (like API keys for income sources) will be encrypted at rest in the database. Regular vulnerability scanning and third-party penetration testing are mandatory. |
| **Denial of Service (DoS)** | An attacker floods the income sync service, running up costs and making the platform unavailable. | - **Mitigation:** Leverage Vercel's and Supabase's built-in DDoS protection. Implement strict rate limiting on all API endpoints and sync jobs. |
| **Elevation of Privilege** | A regular user finds a way to gain administrative access to the system or view another user's data. | - **Mitigation:** The principle of least privilege is enforced everywhere. There are no traditional "admin" roles in the application itself. RLS is the ultimate backstop against this threat. |

## 4. Key Security Controls

- **Authentication:** Handled by Supabase Auth, with MFA enforced.
- **Authorization:** Enforced at the database level via Row-Level Security (RLS). This is our most critical security control.
- **Data Encryption:**
    - **At Rest:** All data in the Supabase database is encrypted by default. Additionally, sensitive fields like `api_key_encrypted` in the `income_sources` table will have application-level encryption.
    - **In Transit:** TLS 1.2+ is enforced for all communication.
- **Secrets Management:** All secrets (API keys, database URLs) are stored in Vercel and Supabase environment variables, never in code.
