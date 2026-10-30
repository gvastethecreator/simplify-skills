# Interface Pass

Read during step 7, only when the repo ships a UI. Review the running application with the available browser or native UI tools. Exercise affected actions and capture the relevant rendered states. The checks below do not require another skill.

Cover the affected routes, views, controls, and meaningful states; expand the scope only when the change or project contract requires it.

## Flow Completeness

- Each user flow completes end to end. No control that leads to a dead or half state.
- Empty, loading, success, warning, and error states exist and are understandable.
- Destructive actions are announced before they happen, and confirmed proportionally.

## Visual Coherence

- Design tokens own color, spacing, radius, elevation. No hard-coded value where a token exists.
- Each supported theme behaves as intended.
- Typography, icons, density, alignment, and surface hierarchy stay consistent across screens.

## Layout

- Responsive down from the primary width. Overflow handled, resizing stable.
- Modal, popover, dropdown, tooltip, and drawer positioning correct, with sane stacking contexts.
- Scrolling has one owner per region. No clipped control, no unreachable content, no accidental layout shift.

## Accessibility

- Semantic HTML first, ARIA only where native semantics fall short.
- Keyboard reachable, visible focus, logical order, modal focus trap and restore.
- Accessible names and roles, sufficient contrast, accessible validation and error text.
- Reduced-motion honored. Nothing depends only on hover, color, or pointer precision.

## Feedback

- Immediate response to user action, clear progress, consistent toasts and alerts.
- No duplicated or contradictory feedback for one event.

## Animation

- Entrances and exits complete their lifecycle. Components do not vanish before exit finishes.
- Correct `transform-origin`, reversible transitions, no stale state after interruption, no loop discontinuity.
- Prefer transform and opacity when appropriate. Keep easing and duration consistent; measure rendering cost when it matters.

## Gallery

When an existing gallery covers changed components, verify the affected variants and states that the change can impact. Do not add unrelated gallery coverage by default.

Do not build a second showcase when one exists.
