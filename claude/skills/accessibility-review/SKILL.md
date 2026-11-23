---
name: accessibility-review
description: Review UI components and pages for keyboard, focus, screen-reader, and contrast problems in both light and dark themes. Use when building or changing interactive components, modals, forms, or toasts, or when asked for an accessibility check.
---

# Accessibility review

Check behavior, not only markup. A component passes when a keyboard user and a screen-reader user can finish its job. Do not claim WCAG conformance without a full audit. Report what you checked and what you did not.

## Automated checks

1. Storybook a11y panel: Miso registers `@storybook/addon-a11y` in `.storybook/main.ts`. Open each changed story and read the Accessibility tab. Record violations by story name.
2. Unit tests: query by role and accessible name (`getByRole`, `getByLabelText`). A failing query means the role or name is missing. Fix the component. Do not switch to `getByTestId` to make the query pass.
3. Miso has no axe-based test layer. Do not add an axe or other a11y dependency without approval.

## Manual keyboard pass

1. Tab through every interactive element in order. Focus must be visible on each one.
2. Enter and Space activate buttons. Arrow keys work inside tabs, menus, and accordions, as their pattern requires.
3. Escape closes modals and popovers.
4. Modal: focus moves in when it opens, stays inside while open, and returns to the trigger on close.
5. Forms: each input has a visible label. Errors link to the input with `aria-describedby`, and the input gets `aria-invalid`. On submit, focus moves to the first error.
6. Toasts: announced through a live region (`role="status"` for polite messages, `role="alert"` for urgent ones). They do not move focus, and they do not trap focus when dismissed.

## Visual checks

- Contrast in light and dark themes. Check token values in the theme file (Miso: `src/theme.css`). Body text needs 4.5:1. Large text and UI component boundaries need 3:1.
- Do not use color alone for state such as errors or selected tabs.
- Respect `prefers-reduced-motion` for any animation you add.
- At 390px width or 200% zoom, content reflows without horizontal scrolling and nothing is hidden.
- Use the browser tool to check the running page, not only the Storybook build.

## Report

- For each finding: the component or story, the check, what happened, and the fix.
- List the areas you did not check.
- Fix problems in the component. Do not fix them in test selectors.

## Rules

- Do not hide problems with `aria-hidden` on focusable content, `display: none` on content that should stay reachable, or `tabIndex` hacks.
- Do not add ARIA roles where a native element already works.
- Do not state a page is accessible without evidence from the checks above.

## Checklist

- [ ] Storybook a11y panel read for each changed story
- [ ] Keyboard pass done on each changed interactive component
- [ ] Focus visible, trapped where required, and restored
- [ ] Contrast checked in both themes
- [ ] Findings reported, unchecked areas listed
