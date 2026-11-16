# Operations and data restoration

Version: 1.0.1  
Date: 2026-11-15

Apply with the [core](../core.md), following [routing](../routing.md). Read when organizing operations, configuring and checking backups or monitoring, rotating secrets, and restoring data. A routine release uses the project's prepared procedures under [deployment](deployment.md); an active incident follows [incidents](incidents.md).

## Runbook and operational organization

Keep one brief operational profile in `docs/runbook.md` or its existing equivalent. Prepare it before production and update it whenever the process changes:

| Arrangement | Minimum content |
|---|---|
| Responsibility | Service owner, alert recipient, and substitute or an explicit statement that there is none |
| Starting work | Manual, event-driven, or scheduled response; who starts the agent and through which available mechanism; which recurring checks CI or a service performs |
| Response limits | Permitted actions and time and cost limits; when the agent reports a blocker. This does not extend the core's production approvals |
| Human unavailability | Channel and method for repeated escalation, safe waiting state; no response is not approval |
| Recovery | Agreed acceptable data loss (RPO) and recovery time objective (RTO); verification method and result |
| Procedures | Links to deployment, rollback, data restoration, secret rotation, and access to logs and monitoring, without secret values |

The owner accepts recovery and support expectations; the agent proposes an option based on risk and cost. Backup frequency and restoration method must meet those expectations; the default daily backup does not guarantee less data loss. Do not promise a response time the selected support arrangement cannot provide.

If no integration starts the agent, record manual operation and a specific responsible person. Sending an alert alone does not mean diagnosis has started. Do not build a separate operations platform without a need; prefer existing monitoring, CI schedules, and project tools.

## Backups

- Back up the database and user files automatically, at least daily and before every production migration.
- Backups are encrypted and stored outside the application server, with another provider or in another location.
- The project sets retention; the default is 30 days.
- Test backup restoration automatically, e.g. monthly, in a temporary environment with production-level access protection and secrets. Block messages to users and production integration effects; allow only the technical access needed to retrieve the backup and observe safely. Delete the environment and data after the test. This is the only permitted exception to the prohibition on copying production data outside production. The report includes the result, restoration time against RTO, and confirmed data recovery point against RPO; the backup file's age alone does not prove completeness. Include no personal data.
- Before making a restored system available, apply deletions and anonymizations performed after the backup date, following [privacy](privacy.md#restoration-after-data-deletion). The restoration test also checks this step. Do not start message delivery or external jobs from restored data before reconciling their current state; restoration must not repeat previous business effects.

## Monitoring and alerts

- Report backend and frontend errors to Sentry or the project's designated tool.
- Sentry integration:
  - `release` is the commit identifier, and `environment` is the environment name;
  - CI creates a Sentry release for every deployment and associates it with commits; connecting Sentry to the GitHub repository helps identify the commit that likely introduced an error;
  - CI uploads frontend source maps to Sentry and does not publish them on the server;
  - reports contain no personal data, passwords, or tokens; disable default user data transmission and filter sensitive fields.
- An uptime monitor checks the production and staging health endpoints externally.
- The staging monitor uses dedicated technical access under the DevOps standard. Do not disable environment protection to enable monitoring.
- Separate application checks through authorized access from checks that deny access to outsiders.
- Alerts use the project's designated channel, email by default, for application unavailability, a sudden increase in errors, failed deployment, failed backup, or detected unauthorized public access to a test environment.

## Agent access to production

- The agent has read-only production access through observation tools: error monitoring, logs, metrics, and deployment status. It has no shell with modification privileges or direct database access.
- A production database query involving personal data is performed by a human or, with their approval, by the agent, read-only and with minimal scope. Do not copy personal data into conversations, tasks, or reports; use identifiers.
- Actions that change production go through the [release](deployment.md) and [rollback](incidents.md#rolling-back-to-a-previous-version) workflows.
