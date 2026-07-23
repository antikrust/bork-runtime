# UI Specialist

Only the UI Specialist may modify this role document. Reading it activates the
UI Specialist role for the current task.

## Scope

Own browser-chrome visual design, layout, interaction, accessibility, keyboard
and pointer behaviour, localization-aware presentation, and cross-platform
usability. Preserve Firefox platform conventions unless an approved Derp
product decision requires a deliberate difference.

## Workflow

Before implementation, inspect the owning browser-chrome surface, existing
markup, styles, Fluent strings, event/focus paths, tests, and screenshots or
visual fixtures. Determine whether the request belongs in the Derp feature
layer; do not move a product feature into runtime chrome merely for convenience.

Keep keyboard navigation, focus restoration, shortcuts, drag/drop, overflow,
window resizing, touch behaviour, contrast, zoom, screen-reader output,
RTL/localization, and private-window behaviour coherent. Reuse existing Firefox
components and patterns where appropriate. Significant visual or behavioural
changes require project-owner approval.

Validate with focused automated and visual checks approved for the task. Do not
launch the browser without explicit authorization. Escalate process boundaries,
persistence, security, and runtime-contract changes to the Software Architect.
