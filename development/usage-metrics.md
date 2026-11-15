# AI Work Metrics and Cost

Version: 1.0.0  
Date: 2026-11-15

Use with the [core](../core.md) only when collecting measurements or reporting agent work time, tokens, and cost. [Progress](progress-reporting.md) describes the regular progress view.

## Measurement data

- Use only provided counters, exports, and timestamps. Missing measurements do not block work; record the gap in one line. Do not build separate telemetry without an assignment.
- Collect from the start of available observation. Do not reconstruct historical tokens from text length, costs from memory, or active time from the difference between two dates. Identify the scope and completeness of the data.
- Prefer an existing source. If a record is needed, use `docs/metrics/usage.jsonl` with source and record identifiers, stage, task, session, executor, actual model, timestamps, and token categories where available. Missing values are `null`, not zero. No prompts, secrets, or personal data.
- Separate elapsed, active, and waiting time. Parallel agent times may exceed elapsed time; do not add them to it. Do not guess waiting time as the remainder.
- Distinguish uncached input, output, cache reads, and cache writes according to the source's semantics. Do not count cache twice if the counter already includes it. Mark unresolved semantics as a limitation.
- Include review, subagents, fixes, and failed attempts. Deduplicate records and use deltas from cumulative counters; resumption does not count history again. Do not add a subagent twice when it is already included in the session total.
- Leave any inseparable part of a measurement unassigned; do not distribute it arbitrarily across tasks.

## Cost and presentation

Separate the amount actually charged, the value of usage included in a subscription, and an API estimate. Do not allocate subscription cost to a project without an agreed method. AI development cost is separate from hosting and product services.

An estimate requires the rates for the appropriate model, token and cache categories, a date, and the official pricing or billing source; include known discounts and additional fees. Do not provide an amount without a basis. Do not add different currencies or a charge and an estimate for the same usage. Optional currency conversion requires an exchange-rate source and date.

At completion or on request, show a table of stages: design, planning, implementation with review and verification, deployment. Provide available time, token categories, and cost with its type, the total, and the unassigned portion. Do not repeat full logs. Provide a forecast as a range only on the basis of representative completed work, never mechanically from the number of tasks.
