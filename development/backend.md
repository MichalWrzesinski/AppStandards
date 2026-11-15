# Backend and integrations

Version: 2.0.1  
Date: 2026-11-15

Apply with the [core](../core.md), according to [routing](../routing.md).

## API conventions

Default conventions apply to new projects; existing contracts and tools remain in place according to the core. Record a significant deviation in one line in the technical profile.

Default to REST/JSON, plural resource names in kebab-case, and a consistent field naming convention appropriate to the ecosystem. Methods and status codes must follow HTTP semantics; reads have no side effects. API dates include a time zone and use ISO 8601 format.

## Error format

By default, a new API uses Problem Details (RFC 9457, `application/problem+json`): `type`, `title`, `status`, a stable machine-readable `code`, and `detail` where needed. Validation returns a list of field errors with a name, code, and message. Clients do not identify errors by parsing text. Preserve the agreed format in an existing API; the security standard defines response security.

## Lists, pagination, and filtering

List results must have a controlled size. For growing datasets, default to database pagination with a page size limit; a small, naturally bounded list does not require a separate mechanism. Default to page-based pagination; use cursors for rapidly changing datasets or when offset costs justify them. Allow filtering and sorting only by permitted fields, with indexes for actual queries. Do not fetch an entire dataset to paginate it in memory.

## API versioning

By default, a new API uses `/api/v1/`. Maintain compatibility with existing clients, including the project's own frontend. Changes to names, types, meaning, required fields, or authentication require an explicit contract migration, by default a new version; prefer a compatible extension before removal. Add new enum values according to client capabilities.

Retiring a version requires an agreed support period, communication, and appropriate headers, e.g. `Deprecation` and `Sunset`. Do not remove an old field or endpoint while dependent clients still use it. The number of supported versions follows actual dependencies, not a universal limit.

## Idempotency

Repeating reads and idempotent operations does not change their effect. Operations whose repetition is harmful, e.g. payments or sending messages, require deduplication; default to `Idempotency-Key` with a stored result and retention period. Reject the same key with different content, by default with `422`, and a concurrent operation still in progress, by default with `409`.

## Database

Default to PostgreSQL. Choose another database or specialized store when it meets the requirements and constraints; record a significant choice in the technical profile. Do not add another store for hypothetical needs.

- Protect integrity with database constraints as well: uniqueness, foreign keys, and value constraints.
- Store time in UTC while preserving appropriate time zone semantics; localize it for presentation. Store monetary amounts as `decimal` or integer units with a currency, never as float.
- A business operation involving multiple records requires a transaction. Do not make external calls inside an open transaction. Protect concurrent changes with record versioning or a lock appropriate to the conflict.
- Avoid N+1 queries, unnecessary data retrieval, and uncontrolled queries in loops. Inspect the execution plan of an expensive query instead of optimizing hypothetically.

### Migrations

Every schema and seed data change goes through a migration in the repository; do not change the schema manually or edit a migration already executed in a shared environment.

A migration must work with the code before and after deployment. Make incompatible changes in expand/contract stages: expand, switch over and migrate data, then remove in a later release. Separate data changes from schema changes, perform large updates in batches, and limit table locks.

Check the migration on an empty database and representative data. Define whether rollback or a forward fix is possible; explicitly mark irreversible deletion. A code rollback is not a data rollback.

## Cache

Add caching when there is a demonstrated need. An entry has a lifetime and an invalidation mechanism, and its key includes the user, language, and permissions if they affect the result. An empty or unavailable cache must not affect result correctness. Default to Redis/Valkey if the project has no solution.

## Background jobs

By default, run long operations and integrations that do not need a result within the request in the background. Jobs must be idempotent, with bounded retries and a visible state after retries are exhausted. A recurring job does not overlap with itself. If an event must correspond to a database write, ensure atomic persistence of the data and the intent to send, by default with an outbox.

## External service integrations

Place integrations behind adapters; provider details do not leak into product logic. Every call has explicit connection and response timeouts, and provider results are validated at the boundary.

A dependency failure produces a controlled outcome: safe deferral (`202`), degraded functionality, or an appropriate dependency error (`502`/`503`/`504`), rather than an unhandled `500`. Use `Retry-After` when retrying makes sense. After a series of failures, limit further calls, e.g. with a circuit breaker.

Retry only transient errors and operations that are safe to repeat. Use a retry limit, increasing delays, jitter, and respect `Retry-After`. Do not retry errors requiring changes to the request or permissions. Do not repeat a financial operation without an idempotency guarantee.

When the outcome after a timeout is unknown, determine the state with the provider, wait for a webhook, or record it for reconciliation. Do not assume failure and blindly repeat the effect.

### Webhooks and integration tests

Verify webhook signatures and freshness, deduplicate events, and do not assume their order. Acknowledge receipt after safely persisting the event for processing; default to background processing. Test timeouts, provider errors, limits, invalid results, and duplicates; use a sandbox or test double, never production effects.

## Logs and health checks

Default to structured JSON on stdout, with a request identifier propagated to jobs and integrations. Use log levels according to their meaning; disable diagnostic details in production. Include no secrets or personal data.

By default, `/health` checks the process without dependencies, and `/ready` checks readiness with essential dependencies. Do not tie process liveness to an optional service failure.
