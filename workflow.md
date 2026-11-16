# Artifact-Driven Workflow

Version: 2.0.1  
Date: 2026-11-15

Use with the [core](core.md), following [routing](routing.md).

State follows from current artifacts and evidence, not the executor's declarations. A stage does not imply a separate agent or session; [execution](development/task-workflow.md#delegating-work) defines delegation. Reuse an existing valid result instead of recreating it from scratch.

## States and transitions

| Transition | Condition |
|---|---|
| Investigation → ready for work | Goal, criteria, boundaries, verification method, and required dependencies are known; no decision blocks this part |
| Ready → implementation | Materials and assumptions are current; execution dependencies and access requirements are satisfied |
| Implementation → verification and review | A coherent change and diff are available. Checks may already run during implementation |
| Verification → integration | Criteria are met, required current CI and independent review of the final commit pass, and Git conditions are satisfied |
| Integration → staging | Environment is prepared and protected, with the correct CI artifact, required access, and approved costs |
| Staging → ready for release | Candidate and configuration are verified, required paths have evidence, migration effects are known, and a failure plan exists |
| Integration → ready for release without staging | Only an existing project that permits this mode; requirements, evidence gaps, and decisions follow the [verification scope](development/deployment.md#verification-scope) |
| Ready → production | Informed human approval for the specified candidate, with deployment conditions met |

Implementation, integration, and production are distinct outcomes. A release may include multiple tasks. A new project prepares the local foundation and CI before integrating code; [setup](development/deployment-setup.md#scope-and-timing) defines when staging is needed. Human assessment of an increment follows [planning](planning/task-planning.md#increment-review).

## Source of state and artifacts

A task has one source and uses the [planning statuses](planning/task-planning.md#statuses-and-a-single-source). Investigation corresponds to `PROPOSED` or `PLANNED`, readiness to `READY`, implementation with checks and review to `IN_PROGRESS`, and integration to `DONE`. Dependent work that has not been integrated may be `BLOCKED`, with an unblocking condition.

Staging and production state belongs to the [release record](development/deployment.md#release-record) and does not change `DONE`. A required fix is a new task. Do not create a second status register for the graph.

Materials, a prototype, a contract, code, a CI result, and an approval are artifacts; not every artifact requires a separate document. Execution defines the checkpoint and report. A blocker stops only dependent work, not the entire assignment.

## Validity and returning to earlier stages

- A requirement or UX change requires reassessing the dependent plan, code, and tests, not the entire product.
- A change to code, configuration, dependencies, or history requires rerunning affected checks and review; pre-merge CI applies to the final commit.
- A change to the candidate or significant configuration requires assessing release evidence and obtaining new approval according to deployments. A cost or access change requires reassessing the scope of authorization.

Results for independent parts may be retained with a justification of their validity. Do not adjust criteria to match an incorrect implementation. On resumption, compare the checkpoint with the actual repository and environment state. Before repeating an external side effect, check its outcome — a timeout does not prove failure.

## When to automate the graph

Start with tasks, Git, and CI. Add validation for recurring errors. A separate orchestrator makes sense when durable resumption or multiple executors are genuinely needed; it must preserve a single source of state, evidence validity, and safe retries.
