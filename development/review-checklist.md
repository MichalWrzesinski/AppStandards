# Reviewer Checklist

Version: 1.4.0  
Date: 2026-11-15

A compact extract of rules for reviewing changes according to the [review standard](code-review.md). The reviewer reads the [core](../core.md), review standard, and this checklist. Read a full detailed standard only for an area of elevated risk affected by the change or when a specific doubt arises. This extract does not change the rules; the source standard takes precedence if they differ.

Before assessing, read the relevant project instructions, overrides, and necessary part of the profile. The default conventions below do not require migration of existing solutions. Distinguish the documented baseline from bugs introduced by the change; an existing vulnerability remains subject to the security standard. Apply items only within the scope to which they relate.

When a source rule changes, check and update its corresponding extract in the same change, preserving significant exceptions. This checklist does not establish a separate policy.

## Always

- Acceptance criteria have evidence for the current commit: tests, CI, recorded browser observations. The author's declaration is not evidence.
- Scope matches the task; no unrelated changes, diagnostic code, dead code, commented-out fragments, secrets, or agent working files such as helper scripts, logs and screenshots ([artifact lifecycle](documentation.md#artifact-lifecycle)).
- The simplest solution meeting the goal: no speculative abstractions, parameters, or layers ([code](code-standards.md)).
- Strict types, no `any`/`mixed`/`Any` without justification; names follow the domain vocabulary.
- Errors: no empty `catch` blocks, broad exceptions caught only at boundaries, each error logged once, messages without internal details.
- Tests verify behavior and negative scenarios, not implementation structure or mocks. A bug fix has a regression test where feasible.
- No weakening of checks solely to obtain a passing result; justified test changes follow requirements, and exceptions follow project policy ([quality](quality-checks.md#integrity-of-quality-checks)).
- A new dependency is justified and maintained; the lockfile is updated.
- Documentation and contract updated in the same change if behavior changes.
- Commits: one purpose, appropriate area, correct title ([Git](git-workflow.md)).

## Backend and data

- Permissions checked on the server for the specific resource; queries filtered by owner; responses use an explicit object with an allowlist of fields.
- Input validated at boundaries; no mass assignment; prices, roles, and owner determined by the server.
- Transactions cover multirecord operations; no external calls inside an open transaction; concurrency is protected.
- No N+1 or uncontrolled queries in loops; growing lists paginated in the database, while small, naturally bounded results do not require artificial pagination.
- Migration compatible with the versions before and after deployment (expand/contract); irreversible data deletion marked.
- Integrations: explicit timeouts, retries only for transient errors and safe operations, controlled outcome instead of `500`, signed and idempotent webhooks.
- By default, new APIs use Problem Details with a stable `code`; explicit profile exceptions and existing formats are retained. Contract changes do not unexpectedly break existing clients.

## Frontend

- Components and tokens instead of slightly different copies and hardcoded values.
- States: loading, empty, error with retry, success.
- Accessibility: semantic HTML, keyboard support and visible focus, labels, contrast, errors associated with fields.
- Communication and client types match the contract; generator according to project needs. No secrets in public variables.
- No console or build errors or warnings in a new project; in an existing project, no new findings introduced by the change, with prior problems identified separately according to the frontend standard.

## Elevated risk

For authentication, permissions, personal data, payments, migrations, secrets, and API contracts, read the relevant part of [security](security.md), [backend](backend.md), or [privacy](privacy.md) and check negative scenarios: access to another user's data, missing permissions, field disclosure in responses, repeated requests, and partial failure.
