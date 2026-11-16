# Critical paths

Version: 1.2.1  
Date: 2026-11-15

Apply with the [core](../core.md) when identifying paths during discovery, assessing task impact, at iteration end, and before release. This is the sole source of critical path rules; other standards refer to it. [Quality checks](quality-checks.md) describe checks for individual changes.

## Definition

- A critical path is a process whose failure prevents basic product use or causes significant consequences, e.g. loss of money or data, or an access violation.
- The process description is in the `Critical paths` section of `docs/product/overview.md`: a stable identifier, e.g. `CP-001`, name, roles, requirements references, and rationale for criticality. Selection follows requirements, not implementation; a path may exist before the application supports executing it.
- A small application usually starts with 2–5 paths. Each additional path must cover a significant risk not covered by the others. Combine actions into a real end-to-end process; do not create a path for every form.
- For a new area, prefer extending an existing path for the same process. Preserve identifiers.

## Verification plan

The plan is in `docs/qa/critical-paths.md`, following the project's language convention. For every path, record: identifier, preconditions (roles, data, services), steps and observable results, task dependencies, the iteration in which the path must be complete, execution method (browser or E2E test), and readiness.

- Refer to product rules instead of copying them. Tests at the appropriate level verify data and error variants.
- In an existing project without a path list, identify the most important processes from documentation; do not reconstruct every feature.
- A business rule change updates requirements. Do not adapt expected results to an incorrect implementation. Development updates readiness and results; it passes scenario gaps to planning.

## Task impact

When planning a task, record identifiers of paths it may affect and the verification scope. Include shared elements: login, navigation, data model, and configuration. Record lack of impact in one sentence. A documentation-only change does not require checking paths.

## Readiness and result

| Readiness | Meaning |
|---|---|
| `Awaiting implementation` | Features needed to execute the entire path are missing |
| `Ready for verification` | All path elements are available |
| `Blocked` | Verification is prevented by, for example, an environment failure or missing data |

| Result | Meaning |
|---|---|
| `Not verified` | No complete verification for the version and environment being assessed |
| `Passed` | All steps were executed and results confirmed |
| `Failed` | Execution revealed a discrepancy |

An unimplemented feature is not a failed result, but prevents treating the path as verified. Record a partial check as partial. Readiness belongs in the plan; the result belongs in the execution report.

## When to verify

- **After a task:** verify the complete path when it is available, the change may have affected it, and existing evidence is insufficient.
- **At iteration end:** verify new and changed paths, and ready paths exposed to risk by changes to shared elements. A missing path that was supposed to be complete in this iteration means the iteration scope is incomplete.
- **Before production:** verify all paths required for the release on the candidate, on staging when required. Every path has a passing result. [Release verification scope](deployment.md#verification-scope) defines the exception for an existing project without mandatory staging: missing evidence remains `Not verified`, with an explicit risk decision, rather than `Passed`. Paths for features explicitly excluded from the release do not block it.
- After a candidate changes, assess impact. You may retain evidence for a path the change could not have affected, with an explicit rationale; repeat verification when in doubt.

## Method and cost

- A path is executed by an agent with a browser, a person, or an E2E test covering the same steps and results in the required version and environment.
- Do not repeat the same check by the author, reviewer, and separate QA agent when a reliable result exists for the same version, scenario, and environment.
- Automate frequently executed paths prone to regression first; do not require automation of all paths from the first iteration.
- Execute complete paths with operations that have business effects on staging or in an equivalent isolated test environment permitted by the project's process; record its limitations as missing staging evidence when it does not replace staging. Production smoke tests do not trigger payments, messages to users, or destructive data changes.
- If available tools cannot verify a path, report the gap and required actions. Do not claim success.

## Recording results

Record aggregate iteration results in `docs/qa/runs/`, e.g. `ITER-01.md`, in the PR completing the iteration; record task results in the task report. Record release candidate verification results in the [release record](deployment.md#release-record), outside the repository, so the record does not change the candidate. Each entry includes: path identifier, date, application version, environment, method, result, evidence reference, and the cause on failure. Browser verification evidence is a short record of observations; E2E evidence is the run report. Do not copy CI results or record secrets, personal data, or full success logs.

After a significant production bug, assess whether to extend a path or its automation before adding a new one.
