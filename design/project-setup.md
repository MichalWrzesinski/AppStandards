# Setting up a new project

Version: 2.3.0  
Date: 2026-11-15

Use with the [core](../core.md) and [routing](../routing.md) when creating a new repository. Do not set up an existing project again. Setup can begin once the name, first increment scope and basic technical decisions are established; it does not require a full specification of every future feature.

## Location and identifiers

- Keep the product name provided by the user. A working name is sufficient for discovery and a prototype; establish the name of persistent resources before creating them.
- The default directory is `~/projects/<ProjectName>`, using the actual home directory read from the environment. Use PascalCase for the directory and repository, e.g. `ProfileHub`, unless the user specifies otherwise. Do not extend this convention to domains, packages or other tool identifiers.
- Create necessary directories without additional confirmation within the task's scope. Preserve existing content; resolve a conflict with another project before writing. Do not bypass permission restrictions.
- By default, create a private GitHub repository under the specified account or the user's personal account. Missing remote access blocks publication and settings, rather than analysis and safe local work. Do not substitute another account without agreement.

## Minimum contents

```text
README.md                  purpose, startup and links
AGENTS.md                  entry point, overrides and project knowledge sources
CLAUDE.md                  import `@AGENTS.md` if the tool does not load AGENTS.md
.gitignore
.editorconfig
docs/
  product/                 first increment requirements
  design/                  approved direction and prototype link
  standards/               complete copy of this set
```

For a new application, use the scaffold specified in the request, user instructions or an available tool instruction, e.g. an application creation skill. Do not look for a scaffold outside these sources or ask about one if none specifies it. If a scaffold is available and meets the needs, use its proven check commands, workflows, deployment and rollback scripts, and runbook template. Confirm the version and operation of necessary safeguards; a template file alone is not evidence. The absence of a scaffold does not block the project: prepare the minimum necessary foundation. Do not remove a standard requirement based on an unverified assumption about a scaffold.

Add architecture, contracts, decisions and tasks when needed; do not create empty documentation. Keep useful prototype code as a starting point, marking its limitations. The implementation foundation adds startup, CI and the necessary deployment artifact.

`AGENTS.md` links to `docs/standards/core.md`, `routing.md` and `workflow.md`, as well as commands, the profile and relevant project knowledge sources. [Documentation](../development/documentation.md#instructions-for-ai-agents) defines its contents and size limit. Other primary tool instruction files may link to it, but preserve local module instructions. Record the set version and copy date; copy the entire set with links, rather than only development files.

## Access and settings

The agent needs access to working branches, PRs and CI, and to integration once its conditions are met. It should not be able to change repository protection or approve production.

Prefer a separate agent identity with minimal permissions, e.g. an account or integration supported by the platform. If an account, token or invitation acceptance is needed, prepare one instruction for a human. Store secrets in a manager or environment, never in the repository or conversation.

If the agent works under the user's account, record the limitation on enforcing separation of permissions and use the deployment standard's fallback production approval mode. Do not claim technical isolation that the plan or account model in use does not provide.

Record settings as an idempotent script, e.g. `scripts/setup-github.sh`, or platform configuration:

- integration through a merge commit according to Git; disable squash and rebase merge by default;
- main branch protection: PR, a required CI check with a stable name, no force pushes or deletion;
- independent review according to the review standard, without mandatory human approval for every PR;
- `staging` and `production` environments: production requires a human decision and prevents self-approval; when the platform plan does not provide this, record a fallback mode according to [deployments](../development/deployment-setup.md#production-approval-mechanism);
- minimal executor permissions, vulnerability alerts and secret scanning to the extent available for the repository.

First prepare a concrete script and check the plan's capabilities. Apply it within explicit authorization for settings; if that authorization or the necessary permissions are missing, request the smallest action needed to proceed. A platform limitation blocks the dependent transition, rather than all local work. Do not weaken required protection to bypass a missing feature.

Tool configuration may allow routine project commands and branch work without questions, but must respect the core. Do not grant general permission for destructive commands, security changes or production approval. Tool restrictions take precedence over a local command allowlist.

## First commit and transition

Check the contents for secrets and unrelated work. The first initialization commit may be made on the main branch before protection is enabled, with the title `[infra] Initialize project with documentation and standards`. This exception covers initial documentation and configuration; do not add an artificial task identifier.

After publication, apply the approved settings and verify their actual operation. Subsequent changes go through PRs, required checks and independent review. Create the CI foundation with the check required by branch protection; its temporary absence does not authorize bypassing integration rules.

Completion condition: materials are preserved, the entry point and rules version are identified, and the repository and safeguards are prepared or their specific blocking issues are recorded. Proceed automatically to preparing the first increment and independent work.
