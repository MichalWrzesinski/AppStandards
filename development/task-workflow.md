# Executing a Change

Version: 3.2.0  
Date: 2026-11-15

Use with the [core](../core.md), [routing](../routing.md), and [workflow](../workflow.md). This standard describes executing an agreed task. A stage does not require a separate agent; the section below determines delegation.

## Delegating work

If tools provide subagents, delegate a task when it meets both conditions:

- it is a coherent unit with its own outcome, usually one PR, a design stage, or analysis of an area;
- it requires reading stage-specific standards, code, or logs that the main agent will not need later.

Do not delegate a small change, a single correction, or work for which the assignment would take longer than execution. Always delegate independent review according to its standard. Select roles for the current need; do not launch a standing team.

The main agent maintains the goal, scope, permissions, dependencies, and communication with the user. When delegating, it does not itself read stage-specific standards or code details needed only by the executor. The subagent makes decisions within its assigned scope but does not expand permissions or replace human decisions defined by the core. It does not communicate with the user: it returns questions in its result and continues work independent of them.

The assignment includes the goal, boundaries, criteria, repository state, permissions, dependencies, expected result, and links to required standard sections and artifacts, not their content or conversation history. The subagent reads the core, the project's main instructions, and the specified sections; routing only when the assignment does not specify sections. It is responsible for diagnosis, checks, and fixes within its scope.

The result is up to a dozen or so lines: decision and rationale, links to changed artifacts, evidence of checks, limitations, questions, and outstanding decisions. Logs and attempt history remain in the subagent's session or CI; information needed for resumption goes into the task checkpoint. The main agent checks the result and dependencies without recreating the analysis or repeating reliable checks unless there is a specific doubt.

Run only independent tasks or work with an agreed contract in parallel. Assign ownership of files or use separate worktrees; coordinate shared files, integrations, and external operations sequentially. The author of an analysis or implementation is not its independent reviewer. Without subagents, continue sequentially, preserving artifacts and a separate review according to its standard.

## Before making changes

- Read the goal, criteria, relevant instructions and materials, and related code. Check the actual work state and preserve existing changes.
- For a small change, a brief record of the goal, scope, and verification method is sufficient. Prepare other work according to [planning](../planning/task-planning.md).
- `READY` confirms the earlier readiness assessment but does not replace checking that materials, assumptions, and execution dependencies are current. Reassess only what has changed or raises a specific doubt.
- A missing profile does not require an audit of the entire project. Add the facts needed for the task according to [input readiness](../planning/input-readiness.md).
- Split excessive scope into coherent increments. Report a blocker for the dependent part; returning to planning does not imply waiting for the human.

## Execution

Follow existing patterns. If the profile does not settle a detail, check nearby code or an analogous case, then the standard and official documentation. Add lasting new facts briefly to the appropriate source; record an unusual decision according to the documentation standard.

Implement, verify, and fix until criteria are met. Do not change requirements independently. Refer a significant discrepancy for clarification; the same agent may return to planning and continue once it is resolved. Reflect an approved requirements change in documentation, tasks, and tests in the same change.

## Out-of-scope problems

You may fix and note a minor bug in a separate commit if all of the following hold:

- it is local, obvious, and easy to verify;
- it does not change a contract, the data model, permissions, or behavior others depend on;
- it does not concern security, money, personal data, or migrations.

Record any other bug, unrelated refactoring, or improvement as `PROPOSED`. Limit refactoring necessary for the task to its scope. If a problem blocks the assignment, present the smallest necessary scope change with a recommendation; do not automatically include all technical debt.

## Diagnosis and escalation

Each repair attempt requires a hypothesis and an observation. After three unsuccessful attempts at the same problem, summarize the evidence and change approach: narrow the reproduction, check assumptions, or select a stronger model if the tool permits it. Do not reset attempt history when handing work over.

Continue if a new testable hypothesis has emerged and the work stays within approved boundaries. Report a blocker when a decision or access is missing, there is no new diagnostic avenue, or further work would exceed the agreed time or cost limit. Do not repeat actions without new grounds. An environment failure does not mean a product decision is needed.

## Interruption and resumption

Update the checkpoint on events: after a commit that changes task state, before delegating or handing work over, when blocked, and at task completion. Do not assume you can predict context loss. A completed task may use its report as the checkpoint, without a second copy.

Preserve in the task source: goal, completed scope, current state, branch or commit, uncommitted changes, check results, blocker, and next action. For a small change without a PR, use a brief persistent note in the project's designated location; do not create an elaborate task just for a checkpoint.

On resumption, compare the record with the repository and sources. Do not repeat an external operation without establishing its actual outcome. Workflow defines evidence validity.

## Completion and report

Maintain statuses and record `DONE` according to [planning](../planning/task-planning.md#statuses-and-a-single-source). If integration access is unavailable, preserve an honest readiness state and identify the condition for the next step.

Save the report with the task or PR according to the [planning template and rules](../planning/task-planning.md#task-file). Mention minor product assumptions only when they are worth assessing in the increment review.

Update overall [progress](progress-reporting.md) if the work has such a report. In the message, state the outcome, verification, and material limitations. Continue the next ready task within the agreed scope; confirmation of every step is an exception defined in the core.
