# Selecting Executor Models

Version: 1.2.0  
Date: 2026-11-15

Use with the [core](../core.md) when actually selecting or escalating the model of a coding agent. [Application AI](ai.md) covers models running inside the application being built.

## Levels and selection

Choose the least expensive available level sufficient to perform the work reliably. Assess total cost: execution, fixes, retries, context handoff, and review. A cheaper model does not justify lower quality or skipped checks.

| Level | Use |
|---|---|
| Economy | Unambiguous mechanical work with low risk; result easy to verify |
| Standard — default | Typical implementation following patterns, tests, routine diagnosis, and review |
| Advanced | High uncertainty, complex dependencies, difficult diagnosis, or significant consequences of errors that ordinary checks do not detect sufficiently |

Assess risk, uncertainty, and verifiability from the available task; do not launch a separate agent for classification. Line count and file type do not determine the level: a few authorization lines or a change to documentation rules may require advanced reasoning.

[Execution](task-workflow.md#delegating-work) defines when to delegate work to manage context. Assign a delegated task a level according to this table. Additionally, do not split a task between levels if handing over context costs more than the difference in rates; in that case, choose the level for the hardest significant part.

## Configuration and limitations

- Maintain the level-to-model mapping in one place in the tool's configuration; the project links to it. Verify current capabilities, availability, and billing in the provider's official documentation when configuring or changing the mapping.
- Check required tools, context, and plan limits. Do not choose a model lacking required capabilities simply because it is cheaper. Adjust the mapping based on the quality of actual results.
- A missing mapping or switching mechanism does not block work with an available, suitable model. Do not invent identifiers or claim a switch without tool confirmation. Record a material limitation in the report.
- A new billing method and exceeding limits require approval according to the core.

## Escalation and handoff

Raise the level when complexity, risk, or ineffective diagnosis emerges. A permissions error, environment failure, or exhausted limit is not in itself a reason to use a stronger model.

Hand over the goal, criteria, current code state, check evidence, attempts, and unresolved problem instead of the whole conversation. A model change does not reset attempt history or review rounds. Do not switch repeatedly without new grounds. Reassess the next independent piece of work without automatically retaining the more expensive level.

Choose the reviewer's level independently according to change risk. It may use the same model as the author but maintains independence according to the review standard. Do not launch multiple reviewers for the same change without a specific need. A stronger model does not replace tests and evidence of behavior.

## Cost assessment

Record the model actually confirmed by the tool and available costs according to [metrics](usage-metrics.md). Mark missing data explicitly; the recommended model is not the model used. Include fixes, review, and handoffs without double counting. Do not describe routine selection after every task; report significant escalation and limitations. Do not claim savings without measurement.
