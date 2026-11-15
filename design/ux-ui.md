# UX/UI and prototype design

Version: 2.0.1  
Date: 2026-11-15

Use with the [core](../core.md) and [routing](../routing.md) for a new product or an area without an established pattern. The goal is to validate the main flow and visual direction before costly implementation. An existing interface pattern usually allows proceeding directly to the frontend standard.

## Direction and human involvement

Read the roles, main flow, scope, content, brand and references. Fill in missing colors, density or style descriptions with a recommendation based on the product; do not require a complete visual design brief. Ask only about preferences that materially change the expected outcome.

By default, prepare one recommended direction and a working prototype of the key flow. Show it to the user on phone and desktop, with realistic copy and representative test data. Do not use “lorem ipsum” or real personal data.

Prepare two or three style variants when the direction is unclear or the user wants a comparison. Add wireframes when complex navigation or layout needs a separate decision; do not automatically require three separate approvals for style, wireframe and final mockup.

## Flow and prototype

- Identify the screens and transitions needed for the main role; account for other roles where they change the process or access. A diagram is needed for non-obvious transitions, rather than as a mandatory copy of the screen list.
- The prototype allows walking through the key process, rather than just viewing static screens. Clearly mark stubs and limitations; a clickable prototype does not prove that the backend works.
- Show material states: empty, error, loading and success. Add an important alternative or edge case if it determines the process's usability.
- The user assesses the direction and flow using concrete material. Collect revisions together; after approval, do not ask again about details derived from that pattern. A materially new flow requires reassessment only of the changed part.
- Prefer a prototype in the target stack, using components that can be reused in the product. Mark experimental code; satisfy the applicable quality and security requirements before integration into production code.

## Brand, components and copy

- Use the existing visual identity. If none exists, prepare a simple logo and icon if the product needs them; branding does not block a functional experiment.
- Save the logo and icon as SVG; generate favicons and application icons only in the required formats. Choose fonts licensed for the intended use.
- Derive tokens and a minimal component set from the approved direction. The frontend must use the tokens, for example as CSS variables; do not maintain a second manual definition in mockups.
- Develop components and their states with subsequent increments. A component catalog page is useful when an existing tool or the size of the set justifies it; do not build a separate documentation application for a few elements.
- Prepare only the requested themes. Establish the tone of copy through a few examples: an error, a confirmation and an empty state. The agent writes the remaining messages in that tone.
- Check the frontend standard's accessibility requirements in the prototype: contrast, keyboard operation, focus, labels and touch targets. Do not present a recommended direction that requires later repairs to basic accessibility.

## Artifacts

By default, save in `docs/design/`: a short description of the approved direction, the prototype source or a link to it, necessary brand assets and material flow decisions. Keep tokens and components where the application uses them; documentation links to them.

Add a screenshot when it helps review or preserve the approved state. Do not require separate PNG copies of every screen when a working prototype and current evidence allow assessment. In an existing project, retain its tools and locations.

## Completion condition

The user has assessed the direction and key flow, the materials are sufficient for the next increment, and material limitations are recorded. The agent develops supporting screens from the established patterns without further approvals. A change to the approved product returns for assessment only within the scope of its impact.
