# New product input brief

Version: 1.0.0  
Date: 2026-11-15

A material template for the [initial assessment of the description](input-readiness.md#initial-assessment-of-a-new-product-description). The user may supply an equivalent description in any form. Do not require every field; “propose a solution” is an acceptable answer, subject to the decision boundaries in the [core](../core.md).

## Information to provide

| Topic | What to describe |
|---|---|
| Goal | What problem we are solving and how we will know the application helps |
| First release | Essential features, exclusions and ideas deferred until later |
| Users and access | Roles, who grants permissions, who sees and changes which data |
| Main processes | End-to-end flow, statuses, material refusals and exceptions |
| Rules | Amounts, currencies, deadlines, limits, deletion and history, if applicable to the product |
| Examples | Concrete input and expected output of main operations |
| Data and integrations | Required information, existing systems, SMTP/API email and available resources; no secrets or real personal data |
| Appearance and language | Application languages, brand materials, interface examples and users' devices |
| Constraints | Mandated stack or hosting, data and environment requirements, known performance needs |
| Autonomy boundaries | What the agent may propose and decide independently, approved services and cost limits, and decisions reserved for the user |
| Gap-filling mode | “I will fill in the gaps myself” or “the agent fills in the gaps and continues”; specifying the mode saves a separate question |

Do not require a business plan, traffic forecasts, database design or complete API. The agent selects technical details. Insufficient data to estimate cost does not authorize inventing a forecast.

## Usage

The description is input, rather than a second requirements register. Maintain findings in the product source according to [discovery](../design/discovery.md#product-documentation), retaining a link to the input material. After filling gaps, do not copy the same rules into subsequent reports. Establish the first release boundary precisely and prepare details for the next increment.
