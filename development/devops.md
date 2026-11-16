# Infrastructure (DevOps) standard for AI agents

Version: 2.2.0  
Date: 2026-11-15

## Scope and application

Apply this standard when creating or changing local environments, Docker images, CI, environment configuration, infrastructure, and automatic dependency updates. It defines how the application is run, built, and checked, and how to prepare it to operate with any provider.

Apply together with the [core](../core.md). [Routing](../routing.md) selects other instructions.

## Governing principles

- **Shared commands.** Every operation has a shared command for humans, agents, and CI; checks may have area-specific variants according to change impact.
- **Everything in the repository.** Environment, CI, and server configuration is recorded in the repository. Do not make manual server changes.
- **One artifact.** Build a release artifact once and use the same artifact in every environment. Environments differ only in configuration.
- **Portability.** In a new project, prefer standard services and limit provider dependencies where this facilitates actual deployment; do not build abstractions for a hypothetical migration.

## Portability

- Pass configuration exclusively through environment variables. Code contains no addresses, hostnames, or environment settings.
- The application container is stateless. Starting another instance or replacing a container does not lose data.
- By default, store user files on server disk, in a persistent volume outside the container, covered by backups.
- Handle files through the framework's established storage abstraction, without direct disk paths, so switching to object storage requires only a configuration change.
- Switch to object storage when the application runs on more than one server or the project specifies it.
- Keep sessions, locks, and queues in a database or cache, not process memory or local files.
- Use standard services available as managed services from every major provider: PostgreSQL, Redis or Valkey, SMTP.
- Put provider-specific functionality in adapters behind an interface. The rest of the application does not depend on the provider.
- Run scheduled jobs as an application process or container, not through server cron.
- The [deployment standard](deployment-setup.md#scope-and-timing) defines hosting and staging selection timing. In an existing project, report only additional requirements arising from the assignment.

## Local environment

- In a new project, use Docker Compose for the application and required services by default. Do not start cache, queues, or stubs unused by the increment. Record a justified simpler approach, e.g. a native tool for a static site, in the profile. Preserve the startup method of an existing project.
- Bind ports exposed on the user's computer to loopback by default, e.g. `127.0.0.1:8080:80`, rather than all network interfaces.
- Do not publish services needed only by containers, e.g. database or cache, on the host without a need.
- Enable local network access or a public tunnel only on the user's explicit instruction, with protection appropriate to the sharing method.
- In a new project, default shared commands are in `Makefile`; preserve existing equivalent project scripts:

| Command | Action |
|---|---|
| `make up` | Starts the project from scratch after cloning: creates `.env` from `.env.example`, builds images, runs migrations and seed |
| `make down` | Stops the environment |
| `make test` | Runs tests |
| `make check` | Runs the full set of checks: formatter, linter, static analysis, tests with coverage |
| `make check-frontend` / `make check-backend` | Checks for the given area if the project has these layers; CI selects them according to change dependencies |
| `make migrate` | Runs migrations |
| `make seed` | Loads demonstration data and test accounts |

- `Makefile` commands call project tools, e.g. `composer` or `npm` scripts. Do not duplicate their logic.
- With containers, Docker and `make` suffice locally. For native operation, document minimum requirements without adding a second startup method.

## Test data and accounts

- `make seed` creates demonstration data showing the main application flows and a test account for each role.
- Document test account credentials in README, stating that they apply only to the local environment.
- Seed is repeatable: rerunning it produces the same state.
- Seed does not run in production.
- Remote test environments do not use publicly documented local passwords. Supply remote test account credentials as environment secrets.

## Docker images

- Use a multistage build. The production image contains no development tools, test dependencies, or source code unnecessary for operation.
- Pin base image versions, never `latest`. Choose official images.
- The container process runs as a non-root user.
- Every image has a `HEALTHCHECK` using the application's health endpoint.
- `.dockerignore` excludes, among other things, `.git`, `.env`, and local dependency directories from the image.
- Tag the image with the identifier of the commit from which it was built. Do not put secrets in the image.

## CI

- Use GitHub Actions by default.
- **On a PR**, CI runs checks appropriate to scope: tests and analysis, secret scanning, API compatibility when the contract is affected, and the relevant artifact build. Audit dependencies when they change and periodically. A documentation-only change may skip the application build, but a required check must assess scope honestly. Do not skip checks of shared dependencies and configuration based only on file extensions. A required summary check assesses impact and the outcome of relevant checks; it marks unneeded scope as not applicable with a reason. Do not require a full `make check` for every change. Unknown impact requires broader checks. Use the selection mechanism from the skeleton if provided; do not redesign it. Verify selection configuration when introducing or changing it.
- **After an application change is merged into the main branch**, CI publishes an immutable artifact, by default a container image, tagged with the commit. Build it once for promotion between environments. For a static site or platform with a different deployment model, the profile defines an equivalent artifact. Documentation alone does not require a new image.
- CI runs the same commands as the local environment so results are the same.
- Cache dependencies and image layers to shorten CI time.
- Pin action versions; pin actions not supplied by GitHub to a commit identifier.
- Grant workflows minimal permissions.

## CI and deployment preparation

- From its foundation, a new application has a local environment, required CI, and artifact publication after merge; do not make these depend on hosting selection.
- Prepare staging CD, target infrastructure configuration, backups, monitoring, and deployment workflows in first-deployment tasks, at the time defined by [deployment](deployment-setup.md#scope-and-timing). Missing CD that is not yet required is not a CI failure; do not create fictitious successful deployments.
- Preserve working CI/CD in an existing project.

## Environments

| Environment | Purpose |
|---|---|
| Local | Agent and human work and tests |
| Remote develop or PR preview, if present | Verification of changes before integration or staging |
| Staging | Remote candidate verification; timing under [deployment](deployment-setup.md#scope-and-timing) |
| Production | Users |

- Staging uses the same application version and operational configuration as close as possible to production, with its own data and secrets. Differences include access, domains, test integrations, and resource scale.
- Do not copy production data to staging or local environments. If needed, use anonymized data. The sole exception is the restoration test environment under [deployment](operations.md#backups).
- A remote develop environment is not required. Do not create one solely because the project has a branch with that name.

## Access to remote test environments

- Staging, remote develop, and PR previews are inaccessible to outsiders by default, even when the application includes public pages or registration.
- Environment access restrictions are additional to the application's own login and permissions. They do not replace them.
- Prefer an access gateway with authentication and an authorized-user list, or access through a private network, e.g. VPN.
- In a simple deployment, HTTP Basic Auth over HTTPS on the reverse proxy is permitted, with strong credentials dedicated to that environment.
- Choose a mechanism suited to hosting, existing tools, and approved costs. Do not introduce a paid service without cost approval.
- Protect every environment address and component: frontend, API, documentation, files, and diagnostic tools.
- Prevent bypassing protection through the direct server address, an alternative domain, a hosting provider address, or an exposed application port. The origin service accepts traffic only through the protected access route.
- Do not expose databases, cache, queues, or technical panels publicly.
- Record protection configuration as code in the repository. Supply passwords, keys, and tokens through the secrets mechanism; do not put them in the repository, URLs, or logs.
- Gateway failure or missing required configuration must not switch the environment to public access.
- Add `noindex` to test environments as a supplement. Do not treat it, `robots.txt`, or a hard-to-guess address as protection.

### Technical access and exceptions

- CI, E2E tests, and monitoring use separate technical access with minimal permissions.
- Do not disable protection for the entire environment during tests or monitoring.
- Health checks performed inside infrastructure do not require public application access.
- Restrict integration exceptions, e.g. provider webhooks, to specific paths and methods. Apply appropriate safeguards, e.g. signature verification, replay protection, and request limits.
- An exception to environment protection does not disable application-required authentication and authorization.
- Document the access mechanism, how permissions are granted and revoked, and every exception with its rationale in technical documentation. Do not record secret values.

## Dependency updates

- Renovate or Dependabot opens PRs updating dependencies, base images, and CI actions.
- Group minor updates and open them at most weekly. Open security fixes immediately.
- A major version update, e.g. of the framework, has a separate PR.
- An update PR follows the normal process: checks, review, and integration.

## Infrastructure as code

- Environment configuration is in the repository. Staging and production configuration, e.g. Compose and a server setup script, is created in first-deployment tasks.
- In larger projects or in the cloud, describe infrastructure using an infrastructure-as-code tool, e.g. Terraform or OpenTofu.
- Preparing a new server or environment is repeatable and documented in the runbook under [documentation](documentation.md#documentation-responsibilities).
