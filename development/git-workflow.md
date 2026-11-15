# Working with Git

Version: 3.1.0  
Date: 2026-11-15

Use with the [core](../core.md), following [routing](../routing.md).

## Permissions and safety

Within an agreed task, create a working branch, commit, push, create PRs, update against the target, merge once conditions are met, and safely delete an integrated branch without asking. Project restrictions take precedence.

- Do not change repository protection or permissions as a side effect of a task. Do not bypass hooks or required signing; resolve the blocker or report it.
- Do not overwrite, revert, or include unrelated existing changes. Respect the user's staged changes; do not use `git add .` or `git add -A` indiscriminately.
- Do not rewrite the history of a target, protected, or shared branch. You may rewrite your own published branch only if nobody else is working on it, checking the expected state (`--force-with-lease`, never a plain force push).
- Do not delete `main`, `master`, `stage`, `develop`, or other protected branches. Reverting code does not guarantee reversal of data changes and external effects.

## Branches and scope

Before making changes, check the branch, staged/unstaged state, and relevant untracked files. Read the target branch, required checks, conventions, and areas from the project. The default branch name is `<type>/<task-id>-<short-description>`; omit the ID if none exists. Types: `feat`, `fix`, `refactor`, `chore`, `docs`. Use an English, lowercase, hyphenated description.

Work on a working branch; if existing changes are on the main branch, preserve them when creating the branch. A PR covers one coherent task or increment. Its description includes the problem, outcome, significant decisions, check evidence, and limitations, without a transcript of the work.

## Commits

A commit has one logical purpose, with directly related tests and documentation. In a monorepo, separate API, client, and admin changes into related commits within the same PR by default. If separation would create a broken state, preserve consistency and briefly explain the exception. Prefer a compatible API extension first, followed by the client.

Do not retain broken states or working checkpoints in the integrated history without explicit justification. Separate an independent fix or cleanup from the feature. Record the task report and status according to [planning](../planning/task-planning.md#task-file), without commits solely to recount review rounds.

Before committing, stage selected files or hunks and inspect the staged diff: scope, completeness, secrets, generated files, and other people's work. Run appropriate checks, create the commit, and check its content and remaining state. Do not claim that a local commit has been published.

## Commit messages

Format: `[area][task-or-iteration] Short change description`. The area tag is required in a monorepo; for a single application, follow the project. Default areas: `api`, `client`, `admin`; additional areas must have a defined scope. Do not invent IDs; omit the second tag if none exists.

Title: English, imperative verb, initial capital after the tags, up to 72 characters, no final period. Add a body for significant rationale and limitations, separated by a blank line and wrapped at 72 characters. Do not repeat the ID from the title. Do not impose Conventional Commits unless project automation requires them.

Do not add `Co-Authored-By` or other AI attribution, including automatic tool attribution, unless the user or project requires it. Record the prohibition according to [agent instructions](documentation.md#instructions-for-ai-agents) so that it takes precedence over the tool's default setting.

## Integration and history

By default, integrate with a merge commit that preserves the task boundary and logical commits, even when fast-forward is possible. Title: `Merge TASK-142: Order cancellation`, or without an ID: `Merge: Invoice date validation`. Do not leave an arbitrary platform-generated title. Squash or rebase merge requires explicit project policy or a justified exception; working fixes for a single step may be combined before review.

Before integration, update the branch against the target: rebase by default for an unshared branch, merge for a shared one. After history changes or conflict resolution, rerun appropriate checks and review. Merge is allowed only with an up-to-date branch, independent approval, and required passing CI for the same final commit. Do not bypass an unmet condition.

After merge, delete the branch locally and remotely only after confirming integration, the absence of untransferred commits and uncommitted work, and the needs of other tasks, worktrees, and people. Do not force deletion after an error without establishing its cause.
