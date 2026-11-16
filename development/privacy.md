# Product privacy and legal requirements

Version: 1.2.2  
Date: 2026-11-15

Apply with the [core](../core.md) when the application processes personal data, uses cookies or analytics, sends data to providers, or prepares its first public release. [Security](security.md) covers technical data protection; this standard covers product obligations and their evidence.

## Limits of the agent's role

- The agent identifies obligations, prepares application mechanisms and draft documents. It does not provide legal advice or declare compliance.
- A human decides the legal basis for processing, the data controller, retention period, and final wording of legal documents. Mark drafts as requiring review before publication.
- A new processing purpose, data category, or data recipient is a decision under the core.

## Data inventory

Maintain a brief register in the sensitive data section of `docs/architecture.md`: data category, purpose, module and table, recipients including providers, processing location, retention period, and deletion method. Update it in the change that introduces new data or a new provider. This is the source for the privacy policy and data processing agreements.

## Providers

- For each provider processing personal data, record its role, processing location, availability of a data processing agreement (DPA), and the basis for transfers outside the EEA, if any.
- Prefer providers processing data in the EEA. A human concludes the agreement; the agent prepares the list and links.

## Application mechanisms

- **Data subject rights:** users can view and correct their data, delete their account, and receive a copy of their data in a readable format, unless the project has established another request-handling process. Deletion includes derived copies, e.g. files and cache; backups expire under retention rules. From the first version, deletion or anonymization implementations keep the operation register needed for [restoration](#restoration-after-data-deletion).
- **Retention:** data without a purpose, e.g. unconfirmed accounts, abandoned files, or old logs, is deleted or anonymized by a scheduled job, with an entry in the same operation register.
- **Cookies and analytics:** cookies necessary for operation do not require consent. Analytics, marketing, and third-party tracking embeds require prior consent unless the solution uses neither device identifiers nor personal data. Prefer cookieless analytics. Refusal is as easy as consent.
- **Forms:** collect only fields needed for the purpose; processing information next to the form links to the privacy policy.
- **AI:** [application AI](ai.md) covers transparency obligations and data transfers to model providers.

### Restoration after data deletion

Restoring an older backup must not return previously deleted or anonymized data to use. Before making the system available, also restore the current state of completed deletions and anonymizations, including derived data, or use an equivalent procedure ensuring the same result.

If using an operation register, keep it outside the dataset reverted to the old backup, with minimal identifiers, access control, and agreed retention sufficient for retained backups. Do not store deleted content in it. Re-execution must be safe. Missing reliable information about these operations blocks making the restored system available again until the responsible person decides how to proceed. The runbook describes the procedure and test under [operations](operations.md#backups).

## Documents before public release

| Document | When | Content source |
|---|---|---|
| Privacy policy | Processing personal data | Data register, provider list |
| Terms of service | User accounts, electronically supplied services, payments | Product rules |
| Cookie notice | Non-essential cookies or analytics | Application configuration |
| Accessibility statement | When required by the service or customer type, e.g. European Accessibility Act, public sector | Accessibility check results |
| Service provider details and contact | Every public service | Information from a human |

Legal pages have stable URLs, are available without login, and are linked in the footer and during registration. Record the version of the terms accepted by the user when acceptance is required.

## Personal data breach

Report a suspected leak or unauthorized access to personal data to a human immediately, including scope, timing, and affected data. A human assesses the obligation to notify the supervisory authority, usually within 72 hours of becoming aware of the breach in the EU. Handle technical work under the [incident procedure](incidents.md#production-incident).
