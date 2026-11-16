# Standards for Building Applications with AI Agents

Standards suite version: 5.0.0  
Date: 2026-11-15

A process for building and developing web applications, including legacy systems. The human defines the intended product and permission boundaries; the agent independently carries the agreed work through integration and prepares the release. Production requires human approval.

## Entry point

1. The agent reads the [shared core](core.md) and project instructions.
2. [Routing](routing.md) selects the path and required standards. Do not read the entire suite for every task.
3. [Workflow](workflow.md) defines transition conditions, evidence validity, blockers, and resumption.

Detailed files apply together with the core. The README is a guide; each execution rule has one home in the appropriate instruction.

## Paths

| Situation | Process |
|---|---|
| New application | Discovery → prototype → technical decisions → setup → implementation increments → staging → production approval |
| Significant new product area | Design only that area → standard increments using the existing stack and process |
| Regular feature or fix | Investigation within the scope of the change → criteria and plan → implementation → checks and review → integration |
| Small, unambiguous change with low risk | Brief record of the goal and verification → implementation → checks, review, and integration; no mandatory iteration or task file |
| First encounter with a legacy system | Learn the assigned area and its dependencies; build knowledge gradually, without mandatory documentation migration |

Risk is independent of line count. A small permissions change or migration does not qualify for the small-change path.

## Instruction structure

| Area | Files and responsibilities |
|---|---|
| Shared | [core.md](core.md): autonomy, precedence, safe work; [routing.md](routing.md): paths and context; [workflow.md](workflow.md): states and evidence |
| Product and design | [Discovery](design/discovery.md): goal, rules, and scope; [UX/UI](design/ux-ui.md): prototype and direction; [technology](design/technology.md): stack, architecture, and contracts; [setup](design/project-setup.md): repository and access |
| Planning | [Input readiness](planning/input-readiness.md): investigation and gaps; [tasks](planning/task-planning.md): increments, criteria, statuses |
| Implementation | [Execution](development/task-workflow.md), [code](development/code-standards.md), [backend](development/backend.md), [frontend](development/frontend.md), [application AI](development/ai.md) |
| Verification and integration | [Quality](development/quality-checks.md), [critical paths](development/critical-paths.md), [security](development/security.md), [privacy](development/privacy.md), [review](development/code-review.md) with the [reviewer checklist](development/review-checklist.md), [Git](development/git-workflow.md) |
| Delivery and operations | [DevOps](development/devops.md), [routine releases](development/deployment.md), [environment setup](development/deployment-setup.md), [operations](development/operations.md), [incidents and rollback](development/incidents.md) — read according to routing triggers |
| New product input | [Brief template](planning/project-brief.md) and [choice of how to fill gaps](planning/input-readiness.md#initial-assessment-of-a-new-product-description) |
| Knowledge and cost | [Documentation](development/documentation.md), [progress](development/progress-reporting.md), [metrics](development/usage-metrics.md), [model selection](development/model-selection.md) |

## Human involvement

Multistage work uses [delegation of coherent tasks](development/task-workflow.md#delegating-work); the main agent coordinates scope and dependencies, while subagents may make decisions within their assigned boundaries.

The human resolves outstanding product decisions, evaluates a concrete prototype, and approves new commitments and production. They do not need to approve every task or implementation detail. Cost approvals may cover categories of actions and limits. The core defines the boundaries, and deployments have additional safeguards.

After each iteration, the human receives a brief, nonblocking review of the increment. The [deployment setup standard](development/deployment-setup.md#scope-and-timing) defines when hosting and staging are needed.

## Applying the standards in a project

- A new project receives a copy of the entire suite in `docs/standards/`, with its version and date. Updating the copy is a separate task.
- The project's main instruction file links to `core.md`, `routing.md`, and `workflow.md`, and identifies overrides and project knowledge sources. It does not copy the standards' content.
- In an existing project, retain the documentation, local instructions, and task system. Map them to the required artifacts; do not introduce a parallel source of truth.

Example prompts:

- “Using the standards at https://github.com/MichalWrzesinski/AppStandards, design and build an application: …”
- “Learn the project within the scope of this assignment: … and complete it according to the specified standards.”
- “Assignment: … Follow the project's standards.”

## Versions and process evaluation

Each standard has its own version and last-modified date. Increase the suite version whenever any standard changes. A change incompatible with the existing way of applying the standards increases the major version; a compatible extension increases the minor version; a correction that does not change rules increases the patch version. New standards start at `1.0.0`. Do not maintain a separate changelog for the standards.

Each rule has one source. Before adding or removing a rule, check whether it changes agent behavior, resolves a deliberate preference, or protects against an observed error; whether a tool already enforces it; and what risk arises if it is not written down. Do not treat a model's familiarity with a rule as evidence that it will apply it reliably. Remove textbook guidance before removing safeguards of unknown origin. Replace a mechanism's detailed instructions with starter code only after confirming that it works.

The agent does not modify the project's copy of the standards. It records a gap, contradiction, or rule that is unhelpful in practice in the project's `docs/standards-feedback.md`: date, standard and section, problem, evidence, and proposal. The human transfers the conclusions to the standards repository.

Evaluate changes on representative work: a new application, a feature in an existing project, and a legacy fix. Compare human interventions, delivery time, fixes following review, missed requirements, and available usage metrics. A lower token count does not prove that quality has been preserved; word count is not a measurement of tokens or cost.

## License

This project is licensed under the [MIT License](LICENSE).

Copyright (c) 2026 Michał Wrzesiński.
