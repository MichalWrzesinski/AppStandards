# Preparing environments and the first release

Version: 1.2.0  
Date: 2026-11-15

Apply with the [core](../core.md), following [routing](../routing.md). Read for the first remote deployment, hosting selection, or changes to security and infrastructure. Complete the first-release checklist before the first production deployment. For routine publication of an existing application, [routine releases](deployment.md) and current project decisions suffice.

## Scope and timing

This section is the sole source of rules for when to select hosting and launch staging; other standards refer to it.

- Establish environment constraints early under the technology standard. Start preparing the first remote environment when increment or integration verification or product assessment requires it, and before production at the latest. This does not require all iterations to be complete; do not make local work depend on purchasing hosting either.
- Preserve infrastructure in an existing project; [routine releases](deployment.md#verification-scope) define required verification scope and the exception for projects without staging. Record a missing environment or CD as `PROPOSED`, with scope and cost; implement after a decision or within already approved scope and budget. Do not restart hosting selection.
- A new product area in an existing project requires assessment only of additional needs and costs.
- If the user has already made a binding hosting provider choice, do not select it again. Before launch, verify suitability for the increment being deployed and the resource cost.

## Assessing the increment being deployed

- Confirm the increment's readiness: implementation, integration, and available local checks. Mark paths dependent on future features according to the QA plan; do not present an incomplete increment as a production-ready version.
- Establish actual needs from code and configuration: API and frontend processes, workers, scheduled jobs, database, cache, queues, persistent files, network connections, and required secrets.
- Use available resource consumption and application behavior measurements. If needed, take a brief, representative local measurement; do not run extensive load tests without a basis in requirements.
- State measurement limitations. Do not derive production throughput or supported user count guarantees from local results.
- Do not require a business plan or revenue, traffic, or growth forecasts. Account for stated constraints and explicitly record other initial configuration assumptions.

## Recommendation and decision

- Check current specifications and prices in the provider's official materials. Recommend the simplest initial solution supporting the required application processes. Present an alternative only if it offers a material difference in cost or capabilities.
- Include staging and production resources, database, persistent files, backups, access protection, and monitoring. Separate fixed and usage-dependent costs; identify exclusions and unknowns, e.g. transfer, domain, or taxes. Do not give a falsely precise total without data.
- Present the solution's limitations and the simplest way to increase resources later. Do not design microservices or extensive scaling infrastructure in advance.
- Record a brief recommendation in `docs/decisions/`. Before purchasing or launching paid resources, confirm that the provider, agreement, and cost fall within existing approval; otherwise obtain a decision under the core. Do not ask for secrets in the conversation.

## Deployment tasks and staging

- After the decision, update the technical profile, plan infrastructure, secrets, CD, staging protection, backup, and monitoring tasks, and execute them under the normal workflow without confirming each task unless an explicit step-by-step mode applies.
- Verify that the required platform safeguards are available in the plan in use. If unavailable, report a blocker with a proposed effective solution; do not claim protection the platform does not enforce.
- Deploy the published image to protected staging. Check migrations, integrations, denial of unauthorized access, and available paths covered by the increment. The routine release standard defines production readiness. Local results do not replace this verification.
- Distinguish in reports between implementation complete, infrastructure prepared, staging verified, and production readiness. Deployment tasks are part of delivering the application.
- Once conditions are met, report production readiness. Selecting and paying for hosting is not production deployment approval; separate human approval is still required.

## Configuring operation coordination

Implement the [operation invariants](deployment.md#operation-coordination) in one mechanism shared by every workflow modifying the resources concerned:

- In GitHub Actions, use the same environment `concurrency` group with `cancel-in-progress: false`. If every pending run must be retained, set `queue: max` in a supported platform version or use an equivalent queue. `false` alone protects the running operation, but the default queue may replace pending runs. [GitHub Actions documentation](https://docs.github.com/en/actions/how-tos/write-workflows/choose-when-workflows-run/control-workflow-concurrency).
- The deployment mechanism must enforce the staging pause during verification; recording it alone is insufficient. Define the pause owner, how to resume after completion or interruption, and session-loss handling. Do not keep a paid runner waiting for a human unnecessarily.
- Across multiple repositories, one repository's CI group is insufficient: use a shared resource lock or a single operation executor. An external lock has an owner, operation identifier, and renewable lease; a timeout alone does not prove the previous executor has stopped modifying data. Before admitting the next operation, confirm the previous executor is absent or technically revoke its ability to write.
- The runbook describes safe identification of a stalled operation and human recovery of the lock. The shared lock also covers manual rollback and backup restoration.

## Material configuration version

Implement determination of the [material configuration version](deployment.md#release-record) in release workflows: a hash of non-secret configuration and secret revision identifiers supplied by the manager, without values or hashes of the secrets themselves. If the manager does not expose revisions, e.g. GitHub Secrets, use the secret name and last-change timestamp from the platform API (`updated_at`); this also reveals manual changes by a human. If the platform exposes no timestamp either, register every secret change as an event in the release record, even if the name and reference are unchanged.

## Production approval mechanism

Requirement: production deployment starts only after explicit human approval of the specified candidate, and the agent cannot grant or bypass that approval. Choose the mechanism to match platform capabilities and record it in project instructions.

| Mode | Mechanism | Who starts it |
|---|---|---|
| Default | Manually triggered `Deploy production` workflow with a protected `production` environment, required human approval, and prevention of self-approval | A human, or the agent at the user's request; the workflow waits for approval |
| Fallback when the platform plan lacks protected environments or the agent uses the user's account | The same workflow, defined on the main branch, triggered manually only, with a condition on the triggering identity; a digest parameter specifies the deployment version, not a branch | Only a human; starting it constitutes approval. The agent never starts it |

- In fallback mode, record in project instructions that separation is a behavioral rule rather than a technical safeguard, and which compensating controls apply, e.g. production secrets available only to deployment workflows. Do not claim protection the platform does not enforce.
- The agent does not approve deployments, bypass the approval mechanism, or deploy to production outside the workflow, e.g. from its machine or via SSH. It does not use a token with deployment approval privileges.

## Checklist before the first release

Before the first production deployment, confirm and record in the release record, as applicable to the application:

- domain, DNS, and TLS certificate with automatic renewal; HTTPS with HSTS; redirects from alternative addresses;
- outgoing mail with SPF, DKIM, and DMARC for the sender domain; a test message arrives and does not go to spam;
- production secrets set through the secrets mechanism; debug mode disabled; seed and test accounts unavailable;
- backup created and test-restored, including reapplication of deletions under [privacy](privacy.md#restoration-after-data-deletion); monitoring and alerts working, confirmed by a test alert;
- production error pages, `robots.txt`, and `sitemap.xml`; `noindex` only in test environments;
- request limits and bot protection active on public forms;
- documents and mechanisms from [privacy](privacy.md#documents-before-public-release) ready, with their content reviewed by a human;
- [runbook and operational organization](operations.md#runbook-and-operational-organization) prepared, including responsibility and the alert response route;
- repository cleaned up according to the [artifact lifecycle](documentation.md#artifact-lifecycle).

An unmet item required for the application blocks the readiness report or is included in the report as a risk accepted by a human.
