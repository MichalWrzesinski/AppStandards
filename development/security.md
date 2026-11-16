# Code security standard for AI agents

Version: 2.1.0  
Date: 2026-11-15

## Scope and application

Apply this standard when writing or changing code that processes input, authenticates users, controls access, stores personal data, handles secrets, or exposes APIs. This document defines how to build safeguards into application code and configuration.

Apply together with the [core principles](../core.md). [Routing](../routing.md) determines the other instructions to use.

Always read the governing principles and the section on security changes outside the task; read other sections according to the change:

| Change | Sections |
|---|---|
| New or changed input: endpoint, form, webhook, file, queue | Validation; injection; HTTP configuration for files and headers |
| Login, session, tokens, passwords | Authentication; enumeration protection; HTTP configuration |
| Data access, roles, new response fields | Access control |
| Personal data | Personal and sensitive data |
| Keys, service accounts, environment configuration | Secrets |

A change affecting several rows requires the sections from each of them.

## Governing principles

- **Security takes precedence over convenience and speed of delivery.** Project conventions do not justify vulnerabilities.
- **Deny by default.** Access, data, and operations remain unavailable until code explicitly allows them.
- **Do not trust the client.** The frontend, mobile application, and any API client can be bypassed. Every safeguard operates on the server.
- **Less data means greater safety.** Collect, store, return, and log only the data necessary for the feature.
- **Use proven solutions.** Use framework mechanisms and established libraries. Do not implement custom cryptography, password hashing, or token handling.

## Secrets

- Do not commit files containing secrets, e.g. `.env`. Commit only a template, e.g. `.env.example`, with variable names and placeholder values.
- Locally, load secrets from an `.env` file excluded in `.gitignore`.
- In CI, store secrets in the platform's secrets mechanism, e.g. GitHub Actions Secrets, separated by environment.
- On the server, supply secrets as environment variables or hosting platform secrets. If the project uses a secrets manager, e.g. Doppler, Infisical, HashiCorp Vault, or AWS Secrets Manager, use it.
- Every environment has its own secrets. Do not use production secrets locally or in tests.
- Do not put secrets in code, logs, error messages, URLs, Docker images, or frontend code. Public frontend variables, e.g. those prefixed with `VITE_` or `NEXT_PUBLIC_`, are visible to every user.
- Keys and service accounts have only the permissions necessary for their task.
- A missing secret, e.g. an API key for a new integration, does not stop work. Locally and in tests, use test doubles or the provider's test environment, add the variable to `.env.example`, and list secrets required on staging and production in the report. Do not request production secrets or accept them in conversation text.
- Treat a secret that reached a repository, log, or another unsecured location as exposed: report it and propose rotation. Removing it from history is insufficient.

## Validation and data integrity

- Validate all input at the system boundary using a mechanism appropriate to the framework, e.g. a DTO with a validator, a Pydantic schema, or Zod. This includes HTTP requests, files, queue messages, webhooks, external service responses, and environment variables.
- Validate against an allowlist: type, format, length, range, and permitted values. Reject unknown fields instead of ignoring or storing them.
- Do not assign request data directly to an entity or model (mass assignment). Explicitly map only fields the user may change.
- Determine values affecting permissions or money on the server, e.g. resource owner, role, price, discount, and status. Do not accept them from the client.
- Check business rules in domain or application logic, independently of form validation.

## Injection and untrusted data handling

- Query the database through an ORM or parameterized queries. Never concatenate queries with input data.
- Encode data displayed in HTML through a template engine or frontend framework with automatic escaping. Do not insert raw user-provided HTML; if necessary, sanitize it with an established library.
- Do not run system commands with input data. If necessary, pass arguments as a list, without a shell.
- Restrict file paths constructed from input to an allowed directory and normalize them before use.
- When fetching resources from user-provided URLs, restrict permitted hosts and block internal addresses (SSRF).
- Do not deserialize untrusted data using mechanisms that can create arbitrary objects, e.g. PHP's `unserialize` or Python's `pickle`.

## Authentication

### Choosing a mechanism

- **By default, for your own browser application on the same site as the API,** e.g. an API under the same domain behind a reverse proxy: use a server-side session. Store a random, opaque session identifier in an `HttpOnly`, `Secure`, `SameSite=Lax` or `Strict` cookie; the server stores the session or its hash, e.g. in a database or cache. Use the framework's mechanism or an established library.
- Use **JWT with a refresh token** when the API serves non-browser clients, e.g. a mobile application or integrations, or when multiple independent services must verify identity. Record the choice in the technical profile.
- In an existing project, preserve the current mechanism; changing it follows the section on security changes outside the task.

### Server-side sessions

- Assign a new session identifier after login and permission changes.
- Apply an idle timeout, e.g. 30 minutes to several days depending on risk, and an absolute timeout. A “remember me” option explicitly extends the session.
- Invalidate the session on the server at logout, password changes, and suspected compromise; a password change may invalidate the user's other sessions.
- Apply CSRF protection according to the HTTP configuration section.

### JWT

- Access tokens are short-lived, e.g. 15 minutes. Refresh tokens are single-use, rotated on every use, and stored on the server so they can be revoked.
- In a browser, store the refresh token in an `HttpOnly`, `Secure`, `SameSite` cookie and the access token in application memory, not `localStorage`.
- Verify the signature, a fixed algorithm (reject `none` and unexpected algorithms), expiry, issuer, and audience. A token contains only necessary data, e.g. a user identifier; anyone can read its contents.

### Passwords and single-use tokens

- Hash passwords with a password-specific algorithm, e.g. Argon2id or bcrypt, through the framework's mechanism. Never store plaintext passwords or encrypt them reversibly.
- Single-use tokens, e.g. password reset or email confirmation tokens, are random, short-lived, single-use, and stored in the database as hashes.

## Access control

- Check permissions on the server for every request, even when the interface does not show the feature.
- Check access to the specific resource, not just the operation type. A user allowed to read invoices may read only invoices they have access to.
- Scope queries to the user's or organization's data at query level, e.g. with an owner filter in the repository. Do not fetch everything to filter it later.
- Use the framework's central authorization mechanism, e.g. Symfony Voters, Laravel Policies, or FastAPI dependencies. Do not scatter permission conditions across controllers.
- Return only fields the user may see through an explicit response object (DTO, serializer with a field list). Do not serialize entire entities or models. Data hidden in the interface but present in an API response is disclosed.
- Protect administrative functions with separate permissions and do not expose them through the same endpoints without role checks.
- For every endpoint with user data, write access-denial tests for another user and for a user without permissions.

## Protection against enumeration and abuse

- Login, password recovery, and similar processes return the same HTTP status, the same response body, and similar response times regardless of whether the email address or username exists in the system. Examples: “Invalid email or password,” “If the account exists, we have sent a message with instructions.”
- Registration may state that an account with the address already exists and point to login and password reset. Because this allows checking whether addresses exist, protect it with stronger safeguards: a low attempt limit per IP address and device, anti-bot verification, e.g. CAPTCHA, after a threshold, and monitoring for unusual numbers of attempts.
- For a resource the user cannot access, return the same result as for a nonexistent resource, e.g. `404`, if disclosing its existence poses a risk.
- Public resource identifiers are unpredictable, e.g. UUIDs rather than sequential numbers. This does not replace access control.
- Apply rate limiting to login, registration, password resets, code verification, message sending, and expensive operations: by IP address and by account or email address.
- After a series of failed login attempts, apply increasing delays or a temporary lockout.
- Exports and bulk operations are restricted by permissions and limits.

## Personal and sensitive data

[Privacy](privacy.md) describes product obligations such as the data register, data subject rights, cookies, and legal documents. This section concerns technical protection.

- Collect only data necessary for the feature. If it can work without a data item or with only part of it, do not collect the entire item.
- Where full data is unnecessary, apply masking, e.g. the last digits of a number or part of an email address, in the interface, API responses, and reports.
- Encrypt particularly sensitive data at field level in the database if its disclosure would be serious, e.g. identity document numbers or health data.
- Design the data model so personal data can be deleted or anonymized without compromising the integrity of other data, e.g. by separating personal data and replacing it with anonymous values.
- Define a retention period for personal data and delete or anonymize it after that period if the project provides for this.
- Do not put personal data in logs, URLs, analytics tools, or error reporting systems. If identification is needed for diagnosis, log an identifier rather than the data.

## HTTP and API configuration

- The application operates exclusively over HTTPS, with the `Strict-Transport-Security` header.
- Set security headers: `Content-Security-Policy`, `X-Content-Type-Options: nosniff`, `Referrer-Policy`, and protection against framing (`frame-ancestors`).
- CORS allows only explicitly listed domains. Do not combine `*` with credentials support.
- When authentication uses cookies, apply CSRF protection.
- Cookies containing session data or tokens have `HttpOnly`, `Secure`, and appropriate `SameSite` flags.
- Check uploaded file types based on content and check their size; store files under random names outside the public directory and serve them through a controlled endpoint.
- Disable debug mode in production. Error responses contain no stack traces, queries, or paths.
- Log security events: failed logins, access denials, exceeded limits, password and permission changes, without passwords, tokens, or personal data.

## Security changes outside the task

Make a change from the following list only when the task explicitly requests and defines it. Otherwise, do not make it; record it as a proposal for a new task with a rationale. If the task cannot be completed without it, stop the dependent part and report the situation with a proposed solution.

- changes the authentication mechanism, token lifetimes, or token storage;
- changes the role and permission model or data access rules;
- weakens or disables a safeguard, e.g. CORS, CSP, CSRF, rate limits, or validation;
- changes cryptographic algorithms, password hashing, or keys;
- changes how secrets are stored and supplied;
- introduces collection of new personal data or transfers to external services;
- changes personal data retention, deletion, or anonymization rules.
