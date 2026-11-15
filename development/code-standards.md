# Writing code

Version: 2.0.1  
Date: 2026-11-15

Apply with the [core](../core.md), according to [routing](../routing.md).

## Conventions and scope

Follow existing conventions and framework tools; default to its standard formatter and style. Do not change style, architecture, or unrelated dependencies as part of a task. Code must solve the current goal without structures, configuration, or abstractions for hypothetical needs.

- Names in code are in English and follow the domain vocabulary. Do not create synonyms for existing concepts.
- Default to single quotes unless the language or formatter requires another form.
- Changes should be easy to trace through search. Do not introduce dynamically assembled names or hidden hooks outside a justified framework convention.
- By default, organize larger applications by functional modules. Add layers and interfaces when they simplify actual dependencies, system boundaries, or tests; do not recreate an entire architecture for a simple case.
- Dependencies, time, and randomness must be controllable in tests; do not hide them in global state.

## Types

Use strict typing for parameters, results, and data with a defined structure. Do not weaken types to silence checks. Allow `any`, `mixed`, and `Any` only for genuinely unknown types at system boundaries, and narrow them before use.

New PHP files require `declare(strict_types=1);`. Add it to an existing file only within the scope of the change and after confirming there is no regression. Use domain types and enums when they protect the meaning of data; do not mechanically add value objects for every field.

## Error handling

- Do not suppress errors with an empty `catch`. Catch general exceptions only at system boundaries.
- Preserve the cause when adding context. Log an error once, where it is handled.
- Distinguish expected domain errors from failures. Convert them into user responses centrally at the boundary, without internal details.

## Comments

A comment explains a non-obvious reason: a rule, workaround, or constraint. It does not repeat behavior evident from the code. Type and documentation generator annotations are permitted when a tool needs them or the language cannot express the information. Do not leave commented-out code or diagnostics.

## Versions and dependencies

For a new project, default to the latest stable, mutually compatible versions; choose LTS if the ecosystem offers it and no newer feature is needed. An existing project retains its versions; upgrading a framework or language is a separate task. Verify new APIs and version compatibility in official documentation; do not introduce deprecated APIs. Remove existing deprecations within the scope of the change without expanding the task.

Before adding a package, check existing dependencies and platform tools. A new package must have a compatible license, active support, and no known vulnerability that prevents its use. Do not add a library for a few simple lines. Explain a significant choice with the change; commit the manifest with its lockfile. Do not update unrelated packages. Report existing unsupported dependencies as upgrade proposals.

Do not manually edit generated code. Perform refactoring necessary for the task as a verifiable step without changing behavior; do not rewrite legacy modules while fixing an issue.
