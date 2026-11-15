# Technology and architecture selection standard for AI agents

Version: 2.3.0  
Date: 2026-11-15

## Scope and application

Apply to technical decisions for a new product or a material architecture change. Check feasibility alongside the prototype before locking in costly assumptions. This document defines how to select the technology stack, architecture, administration panel, data model, API contract and external services, and how to record those decisions.

Use with the [core rules](../core.md). [Routing](../routing.md) determines the other applicable instructions.

## Governing principles

- **Use the default stack unless another is clearly better.** Use the default stack. Choose another only when it better meets the project's needs, and record the rationale.
- **Use the simplest architecture that meets the needs.** Add complexity only when the project requires it.
- **Use established solutions rather than custom ones.** Use established libraries, framework tools and external services instead of building your own equivalents.
- **The agent makes technical decisions.** Commitments and unresolved product consequences remain subject to the boundaries in the core.

## Input

- Product documentation: areas, roles, rules, first release scope, constraints.
- Available prototype, flows and UI direction; do not require all mockups to be complete before assessing feasibility.

## Default stack

| Layer | Default |
|---|---|
| Backend | Python with FastAPI and established ecosystem libraries, e.g. Pydantic, SQLAlchemy, Alembic |
| Frontend | React with TypeScript |
| Public pages | A React-compatible framework with server-side or static rendering, e.g. Next.js or Astro with React components |
| Database | PostgreSQL |
| Cache and queues | Redis or Valkey |
| Administration panel | A panel generated using a backend ecosystem tool, e.g. SQLAdmin for FastAPI |

Choose versions according to the coding standard. Add a cache, queue, panel and separate processes only when needed; the table does not require running every service. In an existing project, retain the stack and architecture unless the task concerns changing them.

### When to depart from the default stack

Choose another technology when it offers a clear advantage for the project, for example:

- the main feature requires an ecosystem that the default stack does not provide in a mature form;
- the application is primarily a content service for which an established content management system exists;
- the client or existing infrastructure requires a particular technology.

Record a material stack departure in one line in the profile: choice and reason. Add an ADR only for a non-obvious choice with material consequences, according to the core. The agent's preference alone is insufficient.

## Architecture

- By default, use a **modular monolith**: one backend application divided into modules corresponding to areas in the product documentation, e.g. `orders`, `payments`.
- Modules communicate through explicit interfaces. A module does not access another module's data directly.
- Do not use microservices unless the project requires independent scaling or deployment of parts of the system and this has been recorded as a decision.
- Background jobs run as a separate process using the same application code.
- By default, the repository combines the necessary applications, e.g. `api/`, `client/`, `admin/`. Do not create separate workspaces or applications that the product does not require.

## Administration panel

- **Small and medium projects:** a panel generated using a backend ecosystem tool, supporting data and administrative permissions. No separate frontend application.
- **Large projects or extensive administrative processes,** e.g. multistage ticket handling, reports or work across multiple teams: a separate application in the `admin/` workspace, built with the same stack as the client frontend.
- The administration panel follows the same security and permission rules as the rest of the application.

## Mobile application

- When the project needs phone support, build a mobile-first web application by default.
- If users are to use the application like an installed app, add PWA support: a manifest, icons and the ability to add it to the phone's home screen.
- Plan a native application only when the project description requires it, e.g. access to device features unavailable in a browser or publication in app stores.

## Data model

- Derive the data model from product documentation: glossary objects, business rules and statuses.
- Entity and field names correspond to established product terms; a separate glossary is needed when it helps domain consistency.
- Apply the backend standard's database rules, e.g. types for time and monetary amounts, and integrity constraints.
- Record necessary model explanations in `docs/architecture.md`; add an ERD when relationships are non-obvious. Do not copy the entire schema that can be read from migrations.

## API contract

- Establish the contract before implementing a dependent client, within the next increment's scope. For a public API or independent teams, use contract-first development; do not specify the entire future API before validating the main flow.
- The project chooses one source of truth: an OpenAPI specification or typed declarations in code. With a source specification, CI compares the implementation against the contract. With code as the source, generate OpenAPI and check compatibility changes; do not maintain a second manually edited contract.
- In a new project, the default published specification is `docs/api/openapi.yaml`, either the source or generated according to the chosen mode. It covers request and response models, material errors and non-obvious examples.
- The frontend may generate types and a client from the current specification. Parallel work requires a stable contract for the relevant increment.

## External services

For sending email, use the existing email service specified by the user by default, through available SMTP or API access. Do not propose a new account, e.g. Brevo, if the current service meets the application's needs. Assess its limits and suitability for the required process; report a missing required feature with a recommendation according to the core. Access is needed before actually sending email, rather than before local implementation with a stub. Supply secrets through environment configuration, rather than the product description.

- For a new integration need, recommend one provider; prepare a comparison of 2–3 when there is a material difference in functionality, cost or constraints. Check:
  - current pricing, fixed fees and limits; estimate the cost of a usage scenario only when the project supplies the necessary data;
  - required features and limitations;
  - data processing location and GDPR compliance;
  - availability of a test environment (sandbox);
  - quality of the official library or API.
- Make choices within the approved provider list and cost limits independently. A new agreement, a new way of processing data or exceeding the approved cost requires a decision according to the core.
- This analysis covers integrations that are part of application functionality, rather than hosting selection. Use stubs or sandboxes locally and in tests; do not activate paid services without cost approval.

## Security in architecture

- Identify personal and sensitive data processed by the application and the modules storing it, as an inventory according to [privacy](../development/privacy.md#data-inventory).
- Identify higher-risk flows, e.g. payments, permissions and integrations, and how to secure them according to the security standard.
- Record this information in `docs/architecture.md`.

## Hosting and operational requirements

- Check known operational constraints early: data region, long requests, workers, scheduled jobs, persistent files, networking and required services. Do not postpone discovering infeasibility until implementation is complete. Where uncertainty is high, run a small technical experiment.
- The [deployment standard](../development/deployment-setup.md#scope-and-timing) defines when to select hosting and staging. Do not require a business plan or traffic forecasts; refine resource sizing with the running application.
- Record known conditions and unresolved dependencies in the profile or a decision. Preserve portability to the extent justified by the project; do not design scaling infrastructure in advance of need.
- In an existing project, assess only the additional resources, constraints and costs of the new area.

## Outcome and transition

Record the technical profile, necessary data model and modules in the architecture, API contract maintenance mode, and material decisions and constraints. Do not create an ADR for every default choice. Materials cover the next increment; later dependencies may remain an outline.

Completion condition: the solution is feasible within known boundaries, the next increment has contracts and dependencies, and commitments requiring approval are resolved before making them. Proceed to new project setup or change planning without further stack approval.
