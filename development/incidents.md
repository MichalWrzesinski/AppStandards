# Production incidents and rollback

Version: 1.0.1  
Date: 2026-11-15

Apply with the [core](../core.md), following [routing](../routing.md). Read for an incident, a failed deployment requiring rollback, or a later rollback. First restore service safely, then remove the cause. Use the current runbook and [production access limits](operations.md#agent-access-to-production); do not load hosting selection instructions.

## Rolling back to a previous version

- Rollback deploys the previous image. Migrations compatible with the running application version allow this without reverting the database.
- The production workflow rolls back automatically after failed verification only after confirming compatibility with the current schema, data, and integrations. Previous approval of an image is not evidence of its current compatibility.
- For a later rollback, e.g. an incident noticed several hours later, the agent starts a separate workflow, e.g. `Rollback production`, without human approval. The workflow accepts only the digest of an earlier production deployment confirmed in the [release record](deployment.md#release-record) and the platform's operation history; it checks its current compatibility. If compatibility cannot be confirmed, it blocks rollback and reports the need for a safe fix.
- After rollback, the agent immediately informs the user of the cause, the version restored, and the application's state.
- Restoring the database from a backup loses data written after the backup. It requires human approval.

Before the operation, use the [shared environment lock](deployment.md#operation-coordination). After rollback, record the new production state and outcome in the operation record and monitoring; do not overwrite the historical release. Data restoration is a separate [operations](operations.md#backups) procedure, not an automatic step in code rollback.

## Production incident

1. **Establish the state:** what is failing, since when, and who is affected. Check alerts, Sentry issues, logs, the readiness endpoint, and the latest deployment.
2. **Inform the user immediately**, briefly covering symptoms, scope, probable cause, and proposed action. If a personal data breach is suspected, also follow [privacy](privacy.md#personal-data-breach).
3. **Restore service:**
   - if the incident began after deployment, start the safe rollback workflow under the rollback conditions; do not blindly revert the image if the schema or data is incompatible;
   - if an external service or infrastructure is the cause, check whether the application has entered degraded operation and report its state to the user.
4. **Do not fix production directly.** Do not change code, configuration, or data directly on the server or in the database. The fix follows the normal change process, taking priority over other tasks.
5. **After service is restored**, add a brief account to the task file for the fix: cause, effects, remedy, and protection against recurrence, e.g. a new test, alert, or process change. Also assess whether the error requires extending a critical path or its verification method.
