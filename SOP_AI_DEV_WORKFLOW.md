# SOP: AI Development Workflow

**Version:** 4.0

This document outlines the standard operating procedure for the AI development team building the **Freelancer Financial Hub**. Our workflow is designed for maximum velocity, enabling a lean team to achieve the productivity of a much larger one and deliver the MVP within the aggressive 12-week timeline.

## 1. Guiding Principle: Velocity & Business Focus

Every aspect of our workflow is optimized for speed and alignment with business goals. As AI agents, we are expected to:

- **Understand the "Why":** Before writing any code, review the relevant sections of the `PRD.md` and `IMPLEMENTATION_BACKLOG.md` to understand the business context and the user problem you are solving.
- **Embrace Automation:** Automate everything that can be automated, from testing to deployment. Manual processes are the enemy of velocity.
- **Communicate Proactively:** Provide clear, concise updates on progress and any blockers. Use a designated Slack channel (e.g., `#dev-log`) for daily stand-up style updates.

## 2. The Development Cycle

1.  **Pick a Task:** Assign yourself a task from the current phase of the `IMPLEMENTATION_BACKLOG.md`.
2.  **Create a Feature Branch:** Create a new branch from `develop` with the naming convention `feature/<ticket-id>-<short-description>` (e.g., `feature/FFH-12-billing-upgrade-page`).
3.  **Develop & Test:**
    - Write the code to implement the feature.
    - Write corresponding unit and integration tests. Our goal is a minimum of 80% test coverage.
    - Run all tests locally to ensure nothing has broken.
4.  **Create a Pull Request (PR):**
    - Push your feature branch to GitHub.
    - Create a PR against the `develop` branch.
    - The PR description must include a summary of the changes and a link to the backlog ticket.
5.  **Code Review:**
    - Another AI agent or a human engineer will review the PR.
    - The review will focus on correctness, adherence to the architecture, and test coverage.
6.  **Merge & Deploy:**
    - Once the PR is approved and all automated checks have passed, it will be merged into `develop`.
    - The merge will automatically trigger a deployment to the `dev` environment.

## 3. Definition of Done

A task is considered **"Done"** when:

- The code is merged into the `develop` branch.
- All automated tests (unit, integration) are passing.
- The feature is successfully deployed and verified in the `dev` environment.
- Any necessary updates to the documentation (`ARCHITECTURE.md`, `API_SPEC_OPENAPI.yaml`, etc.) have been included in the PR.

## 4. Human Approval Gates: The Safety Net

While we prioritize velocity, certain critical actions require human oversight. These gates are designed to prevent catastrophic errors and ensure alignment with business and security requirements.

- **Production Deployments:** Merging a `release` branch into `main` requires a manual approval step in GitHub Actions from the designated Lead Engineer.
- **Destructive Database Migrations:** Any migration that could result in data loss requires a manual review and approval.
- **Changes to Security or Billing Logic:** Any code that modifies authentication, authorization, or the core billing integration with Zoho **must** be reviewed and explicitly signed off on by a human engineer.
