# Selecting a Path and Context

Version: 1.5.0  
Date: 2026-11-15

Use with the [core](core.md). For a new assignment, select a path based on uncertainty and the consequences of error. Do not launch a separate agent for classification. Changing paths during work requires filling in missing decisions only, not repeating completed stages.

## Paths

| Path | When | Required record and process |
|---|---|---|
| Small change | Unambiguous, coherent scope; low risk; known pattern; easy verification; no changes to contracts, the data model, permissions, or commitments | Brief record of the goal, scope, and verification in an existing task or PR description; implementation, checks, review, and integration according to project policy. No mandatory task file or iteration |
| Standard change | Feature, integration, refactoring, or fix requiring investigation of dependencies | Input readiness gate within the scope of the change, task or increment plan, implementation, checks, review, integration |
| New product or significant area | Unsettled needs, rules, or main flows that project patterns do not resolve | Discovery and prototype for the relevant area, technical decisions, setup only for a new project; further work in standard increments |

Assess risk independently of diff size. Authorization, tenant isolation, payments, personal data, migrations, secrets, data deletion, and contract compatibility require appropriate checks even for a one-line change. Doubt about the small-change path means using the standard path, not automatically asking the user.

## What to load

At the start of a session, read the core, routing, and the project's main instructions. Use [workflow](workflow.md) when moving between stages. Then read the necessary sections according to headings and change scope, rather than automatically reading every linked file in full. Within the same session, do not reload an unchanged instruction unless context has been lost. Applicable requirements remain binding; do not skip a section relevant to the risk.

| Work / trigger | Instruction |
|---|---|
| Unsettled product or new area | [Discovery](design/discovery.md) |
| New visual or interaction direction | [UX/UI](design/ux-ui.md); frontend is sufficient when a pattern already exists |
| New stack or architectural decision | [Technology](design/technology.md) |
| Creating a new repository | [Setup](design/project-setup.md) |
| Investigating an existing project, unclear scope | [Input readiness](planning/input-readiness.md) |
| Standard change and larger scope | [Planning](planning/task-planning.md) |
| Executing a change | [Execution](development/task-workflow.md) |
| Code and its verification | Relevant sections of [code](development/code-standards.md) and [quality](development/quality-checks.md); do not redesign tools already established in the project |
| Assessing impact on critical paths, iteration end, release preparation | [Critical paths](development/critical-paths.md) |
| API, data, integrations, workers | [Backend](development/backend.md) |
| Views, interactions, accessibility | [Frontend](development/frontend.md) |
| Input data, access, secrets, personal data | [Security](development/security.md) sections according to the table at its beginning |
| New personal data or provider, cookies, analytics, legal documents, first public release | [Privacy](development/privacy.md) |
| AI models in the application being built | [Application AI](development/ai.md) |
| Changing documentation and project instructions | [Documentation](development/documentation.md) |
| Cleanup at iteration end or before the first release | [Artifact lifecycle](development/documentation.md#artifact-lifecycle) section |
| Commits, PRs, integration | [Git](development/git-workflow.md); when assigning a review, the [Reviewer](development/code-review.md#reviewer) section |
| Reviewing a change as a reviewer | [Review](development/code-review.md), [reviewer checklist](development/review-checklist.md); full standards only for areas of elevated risk |
| Environment, images, CI, infrastructure | [DevOps](development/devops.md) |
| Routine release to a prepared environment | [Deployments](development/deployment.md); project runbook and profile, without rereading setup and operations |
| Hosting selection, first environment, or change to the approval mechanism | [Deployment setup](development/deployment-setup.md); also the routine instructions when executing a release |
| Backup, restore, monitoring, secret rotation, operational responsibilities | [Operations](development/operations.md); deployment coordination section when changing an environment |
| Incident or rollback | [Incidents](development/incidents.md) and the specified sections on operational safety; no hosting selection instructions |
| Adding or changing a feature flag | [Feature flags](development/deployment.md#feature-flags) section; no need to read the entire deployment standard |
| Multistage work and delegation of coherent tasks | [Delegating work](development/task-workflow.md#delegating-work) section |
| Selecting or escalating an executor's model | [Model selection](development/model-selection.md) |
| Overall plan, stage summary | [Progress](development/progress-reporting.md) |
| Measuring time, tokens, or cost of AI work | [Metrics](development/usage-metrics.md) |

This is a list of triggers, not mandatory reading for every task. Documentation-only work does not require reading the backend standard or running the application. Each detailed file applies together with the core; its links do not imply that all linked files must be read.
