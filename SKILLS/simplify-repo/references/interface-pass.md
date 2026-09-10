# Interface Pass

Read during step 7, only when the repo ships a UI. Review the running application, not isolated screenshots. Route depth to `improve-ui`, `better-accessibility`, `review-animations`.

Cover every route, view, modal, panel, form, and meaningful state.

## Flow Completeness

- Each user flow completes end to end. No control that leads to a dead or half state.
- Empty, loading, success, warning, and error states exist and are understandable.
- Destructive actions are announced before they happen, and confirmed proportionally.

## Visual Coherence

- Design tokens own color, spacing, radius, elevation. No hard-coded value where a token exists.
- Light and dark themes both behave.
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
- Transform and opacity over layout-affecting properties. Consistent easing and duration. No repaint cost for decoration.

## Gallery

Every reusable component and meaningful variant appears in the existing gallery, Storybook, playground, or showcase, including default, hover, focus, active, disabled, loading, empty, error, success, long content, compact viewport, theme variants, and animation entry and exit.

Do not build a second showcase when one exists.
