# Work Progress

Version: 3.0.1  
Date: 2026-11-15

Use with the [core](../core.md) for a larger assignment or multistage product. A small change ends with its outcome recorded in the task or PR, without a separate progress report.

## One view

The default `docs/progress.md` summarizes the current scope, not the project's entire history. Statuses come from a single task source; the report links to it and the plan instead of copying their content. Prefer a generated view. Do not create periodic reports, separate LLM sessions, or PRs solely to refresh a counter.

Create the report when first recording larger work persistently, even before the repository exists. Update it during planning, stage completion, integration, a blocker, and scope changes as natural steps in the work. Maintain task statuses, including `DONE` taking effect upon merge, according to [planning](../planning/task-planning.md#statuses-and-a-single-source). Update the view in `docs/progress.md` in the PR that changes state or at the next natural update, with an explicit date and scope of the reported state; do not create a PR solely for the view. Do not start a loop of successive report changes.

A conversation message combines the most important outcome with a progress summary; it does not repeat the full task report. The core defines the language of documents and messages.

## Minimal layout

```text
As of: 2026-11-10 18:00 Europe/Warsaw | scope: release 0.1
Stage: implementation | increment: profile editing
Prepared tasks: 7 DONE / 10 | 1 in progress | 0 blocked
Remaining scope: 2 increments to refine
Now: TASK-008 | next: TASK-009
Release: staging in preparation | production awaiting approval
Metrics: partial data; source: tool export
```

Add a link to the plan and status source. Omit unnecessary fields. The counter is not a percentage of work or time. `PROPOSED` stays outside the plan; show `PLANNED` and unrefined increments separately. When the full scope does not yet have tasks, do not imply that the denominator covers the whole product.

When using iterations, show the goal and sequence, e.g. `1/4 (ITER-00)`; for parallel work, show the actual active tasks. After a scope change, correct the counter and briefly note the difference; a removed task does not become completed. Usage already incurred remains in the metrics.

Separate implementation, infrastructure preparation, and release; they may progress in parallel. The release identifies the candidate, staging, evidence validity, and approval according to workflow. Completed code does not mean production.

Collect and report time, token, and cost measurements according to [metrics](usage-metrics.md) only when the work requires them or the user requests them. One line on measurement availability is sufficient in the progress view.
