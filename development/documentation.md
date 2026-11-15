# Technical documentation standard for AI agents

Version: 3.4.0  
Date: 2026-11-15

## Scope and application

Apply this standard when creating or changing project technical documentation: README, API documentation, technical decision records, and instructions for AI agents. It defines what to document, where, in what form, and when to update it.

Apply together with the [core](../core.md). [Routing](../routing.md) selects other instructions.

## Principle

Update documentation in the same change as the behavior it describes. Record decisions, meaning, and usage that are not apparent from code or configuration. Each piece of information has one source; other locations refer to it. The core defines the language convention.

## Documentation responsibilities

The table assigns responsibilities to stages, not separate agents, and does not prohibit moving between stages. Locations are defaults for new projects; use the project's source mapping in an existing project.

| Part | Default location | Who creates and updates it |
|---|---|---|
| Instructions for AI agents | `AGENTS.md` or `CLAUDE.md` | Development, in the code change |
| README | `README.md`, `<aplikacja>/README.md` | Development, in the code change |
| API contract | `docs/api/openapi.yaml` | Design creates it; development updates it in the code change |
| Architecture: modules, data model, sensitive data, code map | `docs/architecture.md` | Design or inventory creates it; development updates it in the code change |
| Technical profile | `docs/technical-profile.md` | Design or inventory creates it; development updates it in the code change |
| Operational runbook: responsibility, event response, and maintenance procedures | `docs/runbook.md` | Under [operations](operations.md#runbook-and-operational-organization) |
| Technical decisions | `docs/decisions/` | Development, in the code change |
| Product description, business logic, requirements | `docs/product/` | Design and planning; updated when implementing an agreed change |
| UI direction and prototype | `docs/design/` or the project's tool | Design; components and tokens updated with the application |
| Aggregate plan and progress report | `docs/progress.md` | Planning creates the plan; the agent for the current stage updates the view from sources |
| AI work measurements, if available | `docs/metrics/usage.jsonl` or existing tool export, under [metrics](usage-metrics.md) | Available telemetry or the agent using supplied counters; no conversation content |
| Tasks and reports | `docs/tasks/` or an external task system | Planning creates the task; development adds the report |
| Critical path plan and results | `docs/qa/critical-paths.md`, `docs/qa/runs/` | Under the [path standard](critical-paths.md) |
| Candidate and release record | Draft and published platform release, e.g. GitHub Release | Under [deployment](deployment.md#release-record) |
| Standards feedback | `docs/standards-feedback.md` | The agent adds it; a human transfers it to the standards repository |

- Preserve the existing project's structure. Documentation migration is a separate assignment, not a prerequisite for starting work.
- In a new project, documentation is in the repository by default. Tasks, UI design, and other materials may remain in the specified external tool; maintain links and access without manually duplicating sources. Preserve the division in an existing project.
- Update product and design documentation when implementing an agreed change to their content. Do not independently change the meaning of requirements to fit implementation.
- When implementation reveals a material discrepancy, resolve it under the core before dependent work. Return to the necessary planning scope and update the appropriate sources once resolved; do not wait until the final report to raise a blocker.

## Format and README

Write concise Markdown with exact names and relative links. Commands must be ready to run, with a code block language and working directory when it differs from the repository root. Add an example when it resolves ambiguity; a template is an aid, not a list of mandatory sections.

README contains the purpose, minimum requirements, quick start, configuration, and links to commands and further documentation. Explain non-obvious variables from the environment template. Do not copy manifests or the full directory structure. Add a separate application README in a monorepo when its startup or configuration differs from the shared process. The project determines the license.

[Reporting](progress-reporting.md) describes progress on larger work; a small change does not need a separate report. Task or PR results and the telemetry source remain the detailed sources. Do not create empty measurement files.

## API documentation

- Preserve the project's chosen API contract mode under [technology](../design/technology.md). When code is the source, generate OpenAPI using the framework's mechanism. When the specification is the source, update it before dependent implementation and check code compatibility; do not overwrite it unconditionally with generator output.
- Write code so the generator reads the API correctly:
  - typed request and response objects instead of loosely structured arrays;
  - an explicit response model for every returned status, including errors;
  - enumerations for fields with a closed set of values;
  - example values for fields whose format is not obvious.
- When the generator cannot infer an endpoint itself, supplement its description with generator-specific attributes or annotations, e.g. OpenAPI attributes in PHP. Prefer language attributes over comment blocks if the tool supports them.
- Add prose descriptions only where the name and type are insufficient, e.g. a business rule, constraint, or side effect.
- The generated specification is available in the development environment, e.g. at `/api/docs`. If the project commits the specification, update it in the same change as the API.

## Technical decisions

- Record a material departure from a default in one profile line: choice and reason. Create an ADR for decisions with material long-term consequences or a non-obvious tradeoff, not every departure.
- Do not record decisions that follow directly from standards or framework conventions.
- Each decision is a short file in `docs/decisions/`, e.g. `0003-use-uuid-public-ids.md`, with context, decision, and consequences sections. A dozen or so lines usually suffice.
- Mark a decision that no longer applies as superseded and identify its replacement. Do not delete it.

## Instructions for AI agents

- The project has one main agent instruction file, e.g. `AGENTS.md` or `CLAUDE.md`, following the tool's convention. If the project supports several tools, other main files refer to it. Check which file the tool loads automatically. The reference must work as an import, e.g. `CLAUDE.md` containing `@AGENTS.md`; a link alone does not load content. Preserve non-duplicated module- and tool-specific instructions.
- Record overrides of default tool behavior, e.g. a prohibition on AI attribution in commits, in the main file or tool configuration. A standard in `docs/standards/` is not loaded automatically and may not override a default tool instruction.
- The file is loaded in every session, so it is usually no longer than 80 lines. It contains only project-specific information not easily read from code:
  - standards overrides applicable to the project;
  - startup, test, and check commands;
  - non-obvious pitfalls and constraints;
  - project conventions beyond the standards.
- The file contains no change history, task statuses, or content duplicated from README. Library versions and the stack description belong in the profile, except for a very small project as described below. It links to the core, routing, workflow, technical profile, and project knowledge sources; keep commands in one place and link to them if already documented.
- Record a pitfall affecting one module in that module's local instruction file or the profile, not the main file.

### Technical profile

The profile is in `docs/technical-profile.md` and is loaded when a task concerns the area it describes. A very small project may replace it with a few instruction lines. It develops gradually as knowledge is needed; it does not require a complete inventory before the first change.

Include necessary decisions about stack, data, contracts and their source, authentication, UI components, files, CI/CD, and quality checks. Reference manifests and configuration instead of manually copying their versions. Baseline check results must identify the code version and date, rather than imply an always-current state. Record primarily non-obvious pitfalls, commands, and material departures; minor implementation details need no register.

The agent independently adds durable facts established during work and updates the solution description in the same change. It does not change permissions, quality policy, or standards overrides without the appropriate decision. Do not automatically add every detail found in code.

## Artifact lifecycle

Keep a file in the repository only while it has a current reader: a future task, operations or audit. An outdated document misleads more than a missing one; Git history, PRs, CI and the release record preserve the past.

| Category | Examples | Handling |
|---|---|---|
| Living sources | README, agent instructions, product overview, architecture, profile, decisions, API contract, critical path plan, runbook, tokens and components | Keep and maintain |
| Input and design notes | Input brief, discovery notes, prototype findings | Move missing findings to the product or design source, then remove, linking the commit |
| Work history | `DONE` task files, `docs/qa/runs/`, outdated progress view | Remove after the release covering them; keep open tasks. Remove or reduce `docs/progress.md` when no multistage work is active. Do not reuse identifiers |
| Leftovers | Unused prototype code, mocks, diagnostic files | Remove |
| Agent working files | Helper scripts, logs, screenshots, working notes | Keep outside the repository or in `.gitignore`; do not commit |

Clean up briefly at the end of an iteration and fully before the first production release, as a regular task with a PR and review. Do not remove material supplied by the user on your own: propose it in the [increment review](../planning/task-planning.md#increment-review) and remove it after approval. The PR description lists removed and merged files; no separate report.

## What not to document

- Change history: it follows from commit history. Maintain a changelog only if the project requires one.
- Code behavior apparent from names and structure.
- Temporary information, e.g. current task state, in README, product description, architecture, or agent instructions. Record work state in the task; verification results in reports, path readiness in the QA plan, and aggregate progress in `docs/progress.md`.
- The same content in multiple places.
