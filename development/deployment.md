# Routine deployments and releases

Version: 3.3.2  
Date: 2026-11-15

Apply with the [core](../core.md), following [routing](../routing.md). Use when releasing to a prepared environment. Read [setup](deployment-setup.md) when preparing or materially changing infrastructure, [operations](operations.md) for operational configuration or checks, and [incidents](incidents.md) for rollback and incidents. Do not load these instructions for every release if the current profile and runbook resolve the necessary conditions.

Deploy only an immutable artifact published by CI, by default an image from the main branch. In this standard, “image” includes an equivalent artifact recorded in the profile. Every release has a safe failure plan; reverting code does not automatically reverse data changes or external effects.

## Verification scope

A new project requires protected staging before production. An existing project retains its established process: staging is required if it already has it or if project instructions or a human decision require it. Do not treat missing required staging as an exception.

If an existing project has no staging requirement, verify the candidate with the available safe checks of the established process. In the release record, distinguish evidence obtained from missing remote checks and their risks. Before production, a human must explicitly accept the specific gaps; general approval without identifying them is insufficient. A known error introduced by the change or an unmet criterion is not an environment gap and still blocks release. Do not perform unsafe production tests as a substitute for staging.

Propose infrastructure additions under [environment preparation](deployment-setup.md#scope-and-timing). A missing environment does not authorize the agent to expand scope or costs independently.

## Operation coordination

Execute deployments, migrations, rollbacks, and restorations of shared resources sequentially under a shared lock, from the first change through completion of checks and any automatic rollback. Rollback within this operation does not acquire its own lock again. Use the prepared runbook mechanism; [setup](deployment-setup.md#configuring-operation-coordination) describes configuration.

Do not interrupt a migration already in progress with a newer deployment. Do not break a stalled lock yourself: establish the state and report it to a human. A canceled run is not a deployment; a new run confirms current evidence and obtains its required approval without copying the previous approval.

## Staging deployment

The following automatic process applies once hosting and CD are prepared. Staging may already support implementation iterations if risk justifies it and costs are approved. Preserve the working process in an existing project.

- Configure access protection under the DevOps standard before making the environment available. Initial startup must not create a period of public access to an unprotected application.
- After publishing an image from the main branch, CI automatically deploys it to staging.
- Associate verification with the deployment identifier; if staging changes version or material configuration during checks, the result does not confirm the scenario on a single candidate. Pause automatic staging deployments while verifying a release candidate: new merges build artifacts, while deployment waits in a queue until verification finishes or is interrupted. Record the pause in the release record and lift it after verification. Use an isolated environment only with approved cost.
- After deployment, CI checks denial of unauthorized access and runs smoke tests through authorized technical access.
- Failed deployment, access protection checks, or smoke tests stop the change from proceeding to production. The agent treats this as an error to fix in a new task.
- If a check reveals public access to the environment, restrict it before further verification and report the issue. If you cannot restore protection, disable public access to the environment rather than leaving it open.
- Smoke tests may also verify critical paths if they cover all their steps and outcomes under the [path standard](critical-paths.md). Application availability alone does not verify a business process.

## Verifying test environment protection

[DevOps](devops.md#access-to-remote-test-environments) defines protection requirements for staging, remote develop, and PR previews. This section describes their verification.

- After every deployment, check a protected page and a representative API endpoint externally, without a session or credentials. Correct denial is a response appropriate to the mechanism, e.g. `401`, `403`, a gateway redirect, or no network access. A failed connection alone does not prove protection: also confirm that an authorized technical client reaches the working application.
- At first deployment and after network, proxy, routing, or protection changes, check the full scope: all components, bypass routes from DevOps requirements, and integration exceptions (allowed calls work, unsigned calls are rejected). Match checks to the access routes actually configured.
- Perform checks from an outsider's context; a result from a private network does not confirm protection of public endpoints.
- Keep the result in the CI report or release record with version, environment, and scope. Do not repeat the full check if access configuration has not changed.

## Production deployment

### Human approval

Every production deployment requires explicit human approval of the specified candidate and material configuration. The agent does not grant or bypass approval. Use the mechanism recorded in the project; [environment preparation](deployment-setup.md#production-approval-mechanism) describes its configuration. In fallback mode, only a human starts the workflow; in default mode, the agent may start it at a human's request, leaving approval to them.

### Release record

A release candidate has one record outside branch history, by default a draft GitHub Release or equivalent platform record specified in the profile. A record outside the repository requires no commit and therefore does not change the candidate or staging. The record contains the readiness checklist data below, critical path results, staging pause state, blockers with their unblocking conditions, and a reference to platform approval or manual triggering by a human. After production deployment, the record becomes a published release with the artifact digest; confirmed deployment history is the source of rollback targets. A description or Release publication alone does not prove deployment: retain the platform operation identifier, environment, digest, and actual outcome. If record publication fails after successful deployment, reconstruct the record from operation evidence without deploying again.

The **material configuration version** identifies non-secret configuration and secret revisions without their values; the project mechanism described in [environment preparation](deployment-setup.md#material-configuration-version) determines it. Rotating a credential for the same resource under an approved runbook need not change the candidate, but requires a record and appropriate operational checks; changing the resource, account, permissions, or meaning requires reassessment of evidence and approval.

The editable record organizes references; it does not replace enforced approval, CI results, or platform execution history. The workflow checks source evidence, not merely the record author's declaration.

### Deployment readiness

Once scope is integrated and meets the [verification scope](#verification-scope), including required paths under the [path standard](critical-paths.md), the agent reports deployment readiness:

- changes since the last release, e.g. PR titles;
- immutable artifact identifier, e.g. image digest, and associated commit;
- migrations in the release, marking irreversible ones;
- minor product decisions and views designed by the agent, for assessment in the available verification environment;
- verification environment URL and access method, without secrets; where staging is absent, the adopted mode;
- new secrets and configuration required in production;
- aggregate verification result for all critical paths required for the version being released, referencing candidate evidence in the required environment;
- link to the deployment workflow and release record.

Approval covers the specified candidate and material configuration. Changing either requires reassessment of evidence and new approval; do not reuse approval of an earlier candidate.

The workflow receives the artifact identifier established before approval, references to required verification evidence, and the material configuration version. In both approval modes, it shows the same data and checks consistency with approval before changing production. It does not resolve a moving tag, current `main`, or current staging again to select a version. It promotes exactly the verified artifact without rebuilding; any mismatch blocks deployment. A later staging version does not replace an earlier approved candidate.

When staging is required, absence of a passing result for a required path on staging blocks readiness. The exception for an existing project without a staging requirement follows the verification scope above. For the first public release, complete the [first-release checklist](deployment-setup.md#checklist-before-the-first-release); return to it later only for requirements being changed.

### Execution

Before the first resource change, acquire the environment operation lock under [operation coordination](#operation-coordination) and confirm the pinned candidate and approval. Hold the lock until verification and any rollback finish. Manual rollback and backup restoration use the same lock.

The production workflow performs, in order:

1. data backup under the configured [recovery process](operations.md#backups);
2. migrations compatible with the running application version;
3. deployment without downtime: new containers receive traffic only after a successful readiness check;
4. post-deployment verification: readiness endpoint, smoke tests, and error count in the first minutes;
5. automatic rollback to the previous image if verification fails and schema and effect compatibility permit a safe return; otherwise, stop and start the agreed repair procedure;
6. recording the actual operation outcome and its digest in the record and monitoring; publishing a successful release only after confirmed success. Monitoring metadata may be prepared earlier to attribute errors to a failed candidate. After rollback, record the failed deployment and restored version; do not publish the candidate as successfully deployed.

Production smoke tests must not cause real payments, messages to users, or destructive data changes; execute full paths with such effects only in an isolated test environment.
[Incident handling](incidents.md#rolling-back-to-a-previous-version) defines rollback conditions and target selection; do not load diagnosis or data restoration procedures during a successful routine release.

## Feature flags

- Use a flag when an unfinished feature must be integrated before activation, or activation requires a separate decision or gradual rollout. Do not add flags in advance.
- Use the simplest mechanism: environment configuration or a database table. Introduce an external service under the core's cost rules.
- Every flag has a safe default, owner, and removal task after full activation. Tests cover both states if both are included in the release.
- A flag does not replace authorization checks or expand/contract migrations. Enabling a production feature for users is a product decision.
