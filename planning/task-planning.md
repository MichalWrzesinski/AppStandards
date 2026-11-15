# Planning increments and tasks

Version: 4.0.1  
Date: 2026-11-15

Use with the [core](../core.md), following [routing](../routing.md).

## Planning depth

A small change requires only a goal, scope and verification method in the task or PR. No mandatory iteration, identifier or separate file. A standard change has a coherent task with criteria and dependencies. Divide larger scope into increments; prepare the next one in detail, leaving later ones as goals and dependencies without a misleading `READY` status.

Higher risk requires an appropriate verification, migration or compatibility plan. An experiment to reduce uncertainty is allowed, but does not become a finished product without the applicable checks.

## Increments and iterations

By default, split features vertically across the necessary layers into observable outcomes. Technical tasks are justified by a specific dependency, rather than a separate schedule for each layer.

A new project starts with a minimal foundation: a runnable application, environment, CI and artifact, followed by the simplest main flow. An iteration is an optional group of tasks with one goal and a verifiable outcome. An existing project retains its process; first fix only problems blocking dependent implementation or assessment. [Setup](../development/deployment-setup.md#scope-and-timing) determines staging timing.

One task corresponds to one coherent PR in a repository. Multiple repositories require linked PRs and an order of compatible deployments. Size must allow a thorough review, without a mechanical file limit. Order: dependencies → priority → uncertainty and risk → value. The default priority is `medium`; use the project's scale. Parallel work requires independence or an agreed contract and integration.

### Increment review

After an iteration or a material increment, show a human what works and where to view it, material decisions for assessment, limitations and the next goal. This review is non-blocking; continue ready scope without waiting. Handle feedback as a requirements change according to the workflow.

## Criteria and readiness

A `READY` task has a goal and boundaries, observable criteria with a verification method, existing materials, dependencies and check scope, and no open blocking decision. Include negative and edge scenarios appropriate to the behavior; do not add an artificial error case to a spelling fix. “When… then…” is an aid, rather than a required format.

A dependent task may be prepared before its predecessor is complete, but starting requires its execution dependencies to be satisfied. Add an “out of scope” section when there is a real risk of expansion. Link materials instead of copying them.

## Critical paths in the plan

Assess impact according to [critical paths](../development/critical-paths.md). Record only their identifiers and required check scope, or a brief explanation of no impact. Do not duplicate the QA plan.

## Task file

The default is `docs/tasks/TASK-142-order-cancellation.md`; the identifier comes from the project's sequence or task system. Required fields are ID and status; add priority, iteration and other metadata when useful for planning. The project may use an external system instead of files.

Example contents:

```markdown
## Goal and scope
Customers can cancel a paid order before shipment. Partial cancellation is out of scope.
## Acceptance
- Cancellation requests one refund; a repeated request does not request another.
- A shipped order and an order owned by another customer cannot be cancelled.
## Verification
CP-002; provider sandbox and existing authorization tests.
Materials: product cancellation rules and current API contract.
## Report
Criterion results and evidence links; material decisions, limitations and proposals.
```

The report is short, usually up to 15 lines. Record it before final review and update it with fixes, without a round-by-round log. The final SHA, CI result and verdict remain in PR/CI metadata; the report links to evidence. Do not record a commit's own SHA or create a loop of commits adding the previous result. Before creating a PR, use a link to the task or local result, then link them in PR metadata. A pending check is not a success. [Implementation](../development/task-workflow.md#interruption-and-resumption) describes the checkpoint.

## Statuses and a single source

| Status | Meaning |
|---|---|
| `PROPOSED` | A proposal outside the agreed scope |
| `PLANNED` | Agreed scope not yet prepared for execution |
| `READY` | Description ready; starting requires satisfied dependencies |
| `IN_PROGRESS` | Implementation, checks or review |
| `BLOCKED` | Dependent work stopped, with a reason and an unblocking condition |
| `DONE` | Integration into the target branch, rather than deployment |

For files, set `DONE` with the report in the task PR, effective upon merge. An unintegrated file is a proposal and the task remains ongoing; closing a PR without merging does not change the target branch's state. Do not create a separate “mark done” commit. In an external system, the integration or agent updates status after confirmed merge.

Map meanings to the project's system without a second manual copy. A small change may track state in the PR without an artificial file or `READY` status. Include a proposal only according to a decision or a previously approved category of actions.

## Plan changes and visibility

After materials change, assess dependent unfinished tasks; lost readiness means `PLANNED` or `BLOCKED`. Do not change historical `DONE` criteria: new scope is a new task. For larger work, update the progress view; release state is separate. When the next increment is prepared, proceed to implementation without further plan approval.
