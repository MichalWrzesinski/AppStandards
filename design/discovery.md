# Product discovery

Version: 3.1.0  
Date: 2026-11-15

Use with the [core](../core.md), following [routing](../routing.md).

Apply to a new product or only a new area that the application's patterns do not resolve. The outcome is the knowledge needed for the next increment and an agreed boundary for the first release, rather than a complete specification of the future product.

## Exploration and decisions

First apply the [initial assessment of the description](../planning/input-readiness.md#initial-assessment-of-a-new-product-description). Reuse its findings and answers without repeating the same questions. Read the description and materials; identify the problem, users, their main flow and expected outcome. For a detailed description, check for gaps instead of repeating the interview. For a loose idea, propose the smallest verifiable scope with examples.

Keep the name provided by the user. The absence of a final name does not block discovery or a prototype; use an explicit working name and establish the final name before creating persistent identifiers. In an existing project, keep its name and terminology.

Questions concern only unresolved material decisions reserved by the core. Group them into short rounds with a recommendation and a concrete example; record answers in the product source. The agent fills in reversible details. Do not require a question round when the material is sufficient.

## Checking product logic

Within the next increment, check:

- roles, access boundaries and how permissions are granted;
- the lifecycle of important objects, statuses, allowed transitions and the effects of deletion;
- money flows, currencies, rounding and refunds, where applicable;
- deadlines, expiration and time zones;
- material refusals, errors, concurrency and integration failures;
- known legal, data, language and service constraints.

Accompany each identified contradiction with a proposed resolution. The [input gate](../planning/input-readiness.md#common-gaps) defines common defaults, and [privacy](../development/privacy.md) defines data obligations. Questions about later independent scope may remain open.

## First release scope

Propose the simplest main flow from beginning to end, with acceptance criteria and necessary security. A human sets the release boundary. Do not add reports, exports or settings without a need; briefly record later ideas as out of scope.

User stories, story mapping and MoSCoW are optional organizing tools. Use them when they help resolve scope, rather than as three required representations of the same information. The purpose of a feature and its observable outcome must remain understandable without the chosen format.

## Product critical paths

Identify critical processes and describe them according to the [single critical path standard](../development/critical-paths.md#definition). Planning adds verification details.

## Product documentation

By default, a small product has one `docs/product/overview.md`: problem and goal, users and roles, first release scope, material rules and criteria, critical paths, constraints, and later ideas and open questions. Omit sections that do not apply.

As areas grow, extract them into `docs/product/areas/`, and place non-obvious terms in a glossary. Rules and stories referenced by tasks and tests receive stable identifiers. For complex states, use a transition table or diagram without mandatory duplication. Do not recreate all documentation of an existing application for a new module; extend its sources only as needed.

## Extended business analysis

Analyze competitors, the business model, revenue or the market only on explicit request. A commercial product does not trigger this by itself. Do not require a business plan or traffic forecasts; account for the constraints provided.

## Outcome and transition

When the first release scope is established and the next increment has a goal, critical rules and criteria without blocking decisions, proceed to the [prototype](ux-ui.md). Assess product understanding through a concrete flow before planning later increments in detail.
