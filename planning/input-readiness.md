# Input readiness assessment

Version: 3.0.0  
Date: 2026-11-15

Use with the [core](../core.md), following [routing](../routing.md). Identify the scope of a standard change or the next increment of a new product. A small change requires only the relevant checks, without a separate gate or report.

## Access and gaps

Check only the access needed for subsequent actions. Missing GitHub, a remote repository, a copy of the standards, hosting or access to another application does not stop independent analysis and local work. However, do not claim to have completed integration or checks for which access is missing.

| Gap | Action |
|---|---|
| Unresolved material decision about the product or irreversible consequences | Report it with a proposal according to the core; stop the dependent part |
| A detail derivable from sources and patterns, a reversible assumption | Fill it in independently; record only durable information and material assumptions |
| A resource or access needed later | Record the condition for the appropriate transition, e.g. integration or deployment; continue earlier steps |

The goal is to avoid unnecessary questions, rather than guarantee that every future discovery can be predicted.

## Initial assessment of a new product description

Before discovery, assess the supplied description or input file using the [input template](project-brief.md). Check product information needed for the first release; do not require the user to design the architecture or complete every template item.

A gap is material when it concerns a decision reserved for a human in the core or a blocking item among the [common gaps](#common-gaps). Fill other gaps according to the mode.

Read the gap-filling mode from the description or an earlier explicit choice. If material gaps exist, send one message listing each gap, its impact and a recommendation. If the mode is unknown, ask about it in the same message. Do not begin dependent design or implementation before receiving an answer. If there are no material gaps, do not ask about the mode; proceed to discovery.

- User fills in the gaps: retain the gap list with the input material or in the existing product source and end the current run. Resume after the additions are supplied, checking only changes and their impact.
- Agent fills in the gaps: adopt the recommendations in the list unless the user answered otherwise, fill in reversible details and continue discovery without another round on the same matters. Costs, data and permissions remain subject to the core; this choice is not production approval.

Do not report inapplicable options, details covered by adopted defaults or questions about independent later scope as gaps. Discovery reuses this assessment without repeating the interview. This one-time assessment precedes the rules for independently filling gaps during subsequent work.

## New product

Before the next increment, establish:

- the goal and users, scope and observable criteria;
- rules, roles, access and material edge cases needed for this increment;
- languages and known constraints, including legal, data, integration and environmental constraints;
- UX direction and the key flow as a prototype or a sufficient existing pattern;
- technical dependencies and verification method.

The agent supplies the stack, data model, copy and UI details. The API contract is needed before implementing dependent clients within the increment's scope, rather than for the entire future application. Missing traffic forecasts or a business plan do not block work. Unresolved questions about later features do not block an independent increment.

## Existing and legacy projects

Learn the project within the task's scope, starting with the least costly sources:

1. Read applicable instructions, README, the existing task, manifests, commands and CI configuration. Preserve local module instructions and other teams' constraints.
2. Find affected modules and dependencies: routes, contracts, the data model, integrations and related repositories. Prefer framework tools over reading the entire implementation.
3. Read the code being changed and a representative analogous case, together with tests. Check the actual behavior on which the task depends.
4. Run checks needed to establish the change's baseline, and run the application when assessing behavior requires it. Record the version, result and limitations; do not confuse test configuration with proof that tests work.
5. Extend durable knowledge: the technical profile, a module link, a pitfall or a command. Develop the code map gradually; do not describe every module on first contact.

Perform a broader audit of dependencies, change history, coverage and architecture when risk or a separate task requires it. Missing tests do not justify rewriting the entire module; use characterization tests within the planned change's scope.

## Existing project documentation

- Retain the current structure and sources of truth. Record a mapping in the project instructions: requirements, UI design, decisions, API, tasks and check results. Paths in the standards are default locations for new projects.
- Do not move documentation, delete old instructions or replace an active process during onboarding. A necessary migration is a separate task that updates references and accounts for documentation users.
- If behavior is undocumented, reconstruct only the part needed for the change, marking the source and uncertainty. Observing code does not automatically establish a product requirement.
- Do not duplicate tasks in files if the project uses an external system.

## New task

Check the goal, current behavior, whether assumptions are realistic, dependencies and impact on existing users. Establish criteria and necessary materials. An explicit new instruction may change an earlier agreement; ask only about unclear intent or material consequences that have not been agreed. Route a new area without established rules to design only to the extent needed.

## Common gaps

Tasks most often omit the following matters. Check items relevant to the change; do not report each one separately. If the materials do not resolve an item, apply the default unless the column identifies a blocking gap.

| Matter | Default | When the gap is blocking |
|---|---|---|
| Permissions: who may use the feature | Existing role model, least necessary permissions | The feature requires a new role or a change to access rules |
| Existing data | Migration preserves data and its meaning | The change alters data meaning or requires a decision about existing records |
| Deletion | Archive records with financial or legal history using a status; delete others after confirmation | Personal data retention or deletion requirements |
| Edge cases and errors | Behavior according to application standards and patterns: validation, message, retry option | The case concerns money or a business rule |
| Validation and field limits | Reasonable limits for the field type, e.g. text length, date range | A business limit, e.g. maximum amount or quantity |
| External service failure | According to the backend standard: controlled outcome, safe retries only | Inability to operate without the provider requires a decision about limiting functionality |
| Notifications | Only notifications essential to the process, e.g. payment confirmation | New notifications to users or third parties |
| New personal data or a new provider | Minimum data scope, an inventory entry according to [privacy](../development/privacy.md) | A new processing purpose, data category or recipient |
| Change history and audit | Security events according to the security standard | A requirement for user-visible history or a legal audit |
| Reports and export | Do not add unless the task requires them | — |
| Mobile view | Mobile first according to the frontend standard | — |
| Performance and data volume | Pagination, indexes and limits according to the standards | Required volume or response time substantially beyond typical expectations |
| Copy and translations | Copy in the profile's languages, written by the agent | A new application language |
| API compatibility for existing clients | Backward-compatible changes according to the backend standard | A necessary breaking change |
| Data visibility: who will see new data | A role sees only data it previously saw and its own data | A role will see data previously unavailable to it, e.g. other users' data, campaign codes, internal prices |


## Pre-existing problems

Distinguish a baseline problem from a defect introduced by the change using evidence. Fix a broken startup or check before dependent work only when it prevents implementation or reliable assessment of the change. Record unrelated debt as a proposal; do not repair the entire project along the way. A mandatory check that cannot be run still blocks the relevant transition to integration or release.

## Outcome and transition

Record findings in existing materials or the task. In the message, show only gaps requiring a decision, material assumptions and conditions needed later. Do not create a separate gate report. Without blocking issues, proceed automatically to planning or implementation according to the selected path.

Completion condition: the next part of the work has a goal, criteria, a verification method and identified dependencies; it does not depend on an open blocking decision. Independent parts may have different readiness states.
