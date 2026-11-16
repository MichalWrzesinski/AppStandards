# Independent Change Review

Version: 4.1.0  
Date: 2026-11-15

Use with the [core](../core.md), following [routing](../routing.md).

Every change requires independent review of the final commit before integration. Authors do not approve their own work. Review checks behavior against criteria and correctness within the project's boundaries.

## Reviewer

A subagent with clean context performs the review, without the author's reasoning history. It receives the goal, criteria, diff scope, commands, project instructions and overrides, the required profile, baseline, and [checklist](review-checklist.md). It reads relevant sections of full standards for elevated risk or a specific doubt. The PR description may identify scope and evidence, but claims require confirmation.

The reviewer reports findings and does not edit code. Without a subagent, use a new session with the same input; if that is impossible, the review remains unperformed and blocks integration.

## Review depth

- Mechanical change: diff and relevant checks, without running the unchanged application. A CSS-only change follows [impact-based checks](quality-checks.md#checks-based-on-impact), without reviewing unrelated backend code.
- Behavior change: requirements, related code, and current evidence from tests or execution of the flow. A change to documentation rules requires assessing consistency and consequences, not building the application.
- Elevated risk — access, personal data, payments, migrations, secrets, contracts: extended checks appropriate to the risk, including denials, data boundaries, and partial failures.

Do not assess the entire repository. Do not repeat current reliable checks or findings resolved automatically. Verify evidence; a gap or doubt requires follow-up. The author and reviewer do not both need to execute the same scenario again.

## Findings and resolutions

A finding identifies the location, requirement or specific risk, consequence, and proposal. A reviewer's preference is not a bug. Report problems supported by a requirement, evidence, or a credible scenario of consequences. Do not demand a change merely so that the review results in a correction.

| Category | Next step |
|---|---|
| Blocking | Bug, unmet criterion, or rule violation: fix before integration |
| Minor | Local noncompliance with a requirement or applicable rule: the author fixes it, and the reviewer confirms using only the fix's diff; preferences are suggestions |
| Suggestion | Optional improvement: the author's decision or a separate proposal; does not block approval or require another round if no change is made |
| Human decision required | Unresolved significant product or permissions issue according to the core |

Resolve technical disputes using criteria, evidence, and the standard; for equivalent alternatives, choose the simpler and safer one. The author does not independently dismiss or reclassify a blocking finding. After three rounds without resolution, summarize the problem and change the diagnostic approach according to execution; do not refer a technically resolvable dispute to the human.

## Completion condition

Met criteria, sufficient current evidence, and no outstanding required fixes allow the change to be approved, including with no findings at all. Do not expand the review after these conditions are met without new grounds. A missing required check remains missing evidence, not a passing result.

## Fixes and outcome

A subsequent round receives the previous findings and the new diff. The reviewer assesses fixes and their impact without rereading the entire unchanged scope. A report or status alone also requires a brief independent review of its accuracy, but not repetition of unrelated application tests. A criteria change is not mechanical merely because it does not change code.

The change must be small enough to assess thoroughly as a whole, without an arbitrary line limit. The reviewer may block a heterogeneous or excessively large scope with a proposal to split it. Separate mechanical changes from logic when this makes verification easier.

Verdict: **approved**, **changes requested**, or **human decision required**, with the commit, scope of checks, findings, and limitations. Each subsequent commit requires an assessment of its impact. Approval and required passing CI for the final commit allow integration once Git conditions are met, without additional human approval.
