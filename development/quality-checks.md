# Quality checks

Version: 3.2.0  
Date: 2026-11-15

Apply with the [core](../core.md), according to [routing](../routing.md).

## Scope of checks

Before implementation, define observable criteria and how to verify them. Select checks based on the change and its dependencies, not line count. Use project commands; test results do not replace a build, type analysis, contract checks, or UI observation when the change requires them.

Start with targeted checks. Before committing, check the affected area; before final review, run the complete set required for the scope; before merging, obtain current required CI results. Do not repeat valid results without a reason; a new change requires rerunning checks it may have affected. Associate evidence with the version, scope, and environment.

Limit tool output to what is needed: exit status, summary, errors, and warnings relevant to assessment. Read full logs only for diagnosis; do not paste success logs into context or reports.

Do not require code tests for spelling or formatting changes. Appropriate diff, document, or preview checks still apply. [Critical paths](critical-paths.md) define the rules for complete product processes.

## Checks based on impact

A CSS-only change with no impact on contracts, server logic, dependencies, or backend configuration does not require analysis of all Python/PHP code or backend tests. Check the diff, relevant style checks or frontend build, and the affected view at appropriate sizes, accounting for accessibility impact. Match UI verification depth to the actual change; do not run complete application processes for a local visual fix that does not affect their behavior.

Shared styles may affect many views. Configuration, dependency, or build process changes may require broader checks despite a small diff. Determine scope from dependencies, not file extensions alone.

Narrow the agent's local checks according to impact. Required CI runs according to its configuration: do not disable or manually skip it. If CI has no selection mechanism, the full run remains required; you may propose selection as a separate change according to [DevOps](devops.md#ci).

## Static analysis, formatter, and linter

A new project has an automatic formatter, linting, and strict type checking appropriate to its ecosystem, run locally and in CI. In an existing project, preserve its tools, levels, and agreed baseline; new code must not add errors to the baseline.

Make small tooling additions within the task's scope. Record a significant change to quality infrastructure separately; a missing required check blocks the relevant transition, not independent work. In legacy code without an established level, define a feasible starting point rather than forcing repairs across the entire project.

## Code coverage

Preserve the project's threshold and policy. A new project selects them according to risk, without a universal threshold for every layer. Tests of significant rules, data boundaries, money, and access denials are required. Executing lines without assertions about behavior is insufficient.

Exclusions must be narrow, substantively justified, and consistent with policy; the reviewer assesses them. Difficulty reaching a threshold does not justify an exclusion. Do not fix historical legacy coverage incidentally to a task, but do not reduce regression protection.

## Creating and maintaining tests

- For a fix, add a regression test where feasible: it reproduces the error before the change and passes afterward. Record if this cannot be verified.
- Before changing non-obvious legacy code without tests, add characterization tests within the change's scope. Do not encode a recognized bug as the expected result.
- Test behavior derived from requirements, important boundaries, and errors. Choose unit, integration, or E2E tests according to risk; TDD is a default aid for logic and bugs, not a requirement for every change.
- Tests must be independent and repeatable. Substitute time and randomness; avoid fixed delays. Do not create tests solely for implementation structure, confirming mocks, or coverage results.
- Use test doubles at system boundaries, not to hide problems in your own integration. Verify data integrations against a real service of the same type as production; verify providers using sandboxes or controlled test doubles.

## Integrity of quality checks

Do not remove or skip tests, weaken assertions, analysis levels, thresholds, or required CI checks, or expand the baseline and exclusions merely to obtain a passing result. A test change must follow an agreed requirements change. The implementation alone does not justify a new expected result.

A justified exception requires a substantive reason, narrow scope, and consistency with project policy. Do not change that policy independently. Do not rerun a flaky test until its first success while ignoring previous failures; record the instability and assess its impact.

## Pre-existing errors and blocking issues

Distinguish regressions, pre-existing issues, and environment failures based on evidence. Use CI or a safe, isolated comparison with the baseline version; do not revert others' or current changes. Without a determination, the cause remains unknown.

An unavailable tool, database, or browser does not prove a code error, but prevents claiming success. Mark an unperformed required check as blocked and continue independent checks.

## Frontend verification

Verify interaction or layout changes in a browser: the main flow, significant alternatives and states, appropriate screen sizes, keyboard and focus, console, and requests. Use an available accessibility scanner without treating it as a complete audit.

A current E2E test may provide evidence for aspects it covers; a screenshot does not prove a process works, and a build does not prove interactions work. Without a browser, perform available checks and explicitly state the lack of visual and interaction verification.

## Higher-risk changes

For permissions, tenant isolation, money, personal data, migrations, data deletion, secrets, and contracts, extend checks as appropriate to include denials, backward compatibility, concurrency, transactions, idempotency, timeouts, and partial failures. Assess safe deployment and reversibility of effects; passing tests do not authorize an irreversible production operation.

## Verification environment safety

Confirm the environment before a destructive command. Tests use isolated data and services without real payments, messages, or deletion of others' resources. Do not test on production data; a controlled backup test follows [operations](operations.md#backups). Clean up only your own resources.

## Verification completion procedure

Every criterion has a status: **met** with evidence, **unmet**, **unverified** with the missing evidence, or **not applicable** with a rationale. Do not change criteria independently. The task report links to results and identifies limitations, pre-existing issues, and errors introduced by the change, without full success logs or concealing significant warnings.

A lack of budget or time does not authorize silently skipping checks. Incomplete verification allows preserving work but blocks a declaration of readiness for integration. The result, current review, and CI must meet the [workflow conditions](../workflow.md).
