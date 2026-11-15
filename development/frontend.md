# Frontend

Version: 2.0.1  
Date: 2026-11-15

Apply with the [core](../core.md), according to [routing](../routing.md).

## Visual consistency

Use the project's components, tokens, and tone. Default to mobile first, without hiding features on phones unless requirements justify it. A mockup does not require mechanical reproduction, but shared elements must behave consistently.

Before adding a component, check for an existing variant. Visual values belong in tokens, not scattered copies. Default to one styling approach, without inline styles except for dynamic values. Choose a UI library to meet interaction needs; do not build complex elements from scratch merely because the application is small.

Resolve unspecified, reversible view details using established patterns, without asking. A substantially new flow follows UX/UI. In the report, list only independent decisions worth the user's assessment, not every default spacing value and text.

## Accessibility

New UI targets WCAG 2.2 AA. Use semantic HTML, labels, keyboard support, and visible focus. Default to touch targets of at least 44 × 44 px; text contrast of at least 4.5:1, and large text and UI element contrast of 3:1. Do not convey information through color alone; images have appropriate `alt` text, empty for decorative images. ARIA supplements semantics rather than replacing them unnecessarily. Respect `prefers-reduced-motion`.

The layout must work at relevant screen sizes without forced horizontal scrolling. Account for the on-screen keyboard and safe areas in forms. [Quality checks](quality-checks.md#frontend-verification) define accessibility and UI verification.

## Forms and states

Client validation improves UX; the server still determines validity and permissions. Rules must match the contract. Associate server errors with fields, link messages to fields for screen readers, and preserve entered data. Prevent duplicate submission during processing and show its state.

A data-driven element handles appropriate states: loading, empty, error, and success. An error allows retrying when safe; an empty state suggests a useful action. Avoid layout shifts during loading. Do not leave a blank screen or an unhandled request.

## API communication

Default to a shared API client layer with types generated from the contract if the project generates them or this prevents significant duplication. A simple client does not require introducing a generator without justification; type compatibility and error handling still apply. Handle network errors, timeouts, lack of permissions, and session expiry consistently. Use an existing data fetching and caching library when needed; do not build an equivalent.

New UI has no console, build, or analysis errors or warnings. In an existing project, the change adds none; report pre-existing issues separately.

## Text and translations

A monolingual project with no expansion plan does not require a translation library. Maintain repeated messages centrally. A multilingual project defaults to translation files and an i18n library. Format dates, numbers, and monetary amounts according to locale settings.

## Public pages and performance

Public content should be available in HTML through SSR or static generation; authenticated areas may be client-rendered. Public pages have appropriate titles, descriptions, and canonical URLs; add a sitemap, robots directives, sharing metadata, and structured data as appropriate to the published content.

For public pages, aim for Core Web Vitals: LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1. Choose images and fonts for their use; do not load heavy routes and elements before they are needed. Control JavaScript size; justify optimization by its impact, not merely its possibility.
