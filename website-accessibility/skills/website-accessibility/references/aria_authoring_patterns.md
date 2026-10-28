# ARIA authoring patterns reference

Canonical patterns for the most-frequently-built custom widgets. Cross-referenced to the W3C ARIA Authoring Practices Guide (APG): <https://www.w3.org/WAI/ARIA/apg/>

## Guiding rule

**The First Rule of ARIA: if a native HTML element does the job, use it  -  don't reach for ARIA.**

- `<button>` beats `<div role="button">` because it gives you keyboard activation, focus management, and screen-reader semantics for free.
- `<a href>` beats `<div role="link">` because it gives you focus, keyboard activation, and correct middle-click behavior for free.
- `<select>` beats `role="listbox"` for a simple menu  -  it gets native mobile and keyboard behavior on every platform.

Use ARIA when native HTML genuinely cannot express the widget: comboboxes with autocomplete, tab strips, tree views, complex disclosure widgets, live regions.

## Modal dialog

```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
  <p>Are you sure you want to delete this file?</p>
  <button type="button" data-close>Cancel</button>
  <button type="button" data-confirm>Delete</button>
</div>
```

Required behaviors:
- On open: move focus into the dialog (first focusable element, or the dialog itself with `tabindex="-1"`).
- While open: trap focus inside the dialog.
- On Escape: close the dialog, return focus to the element that triggered it.
- Background: mark with `inert` (modern) or `aria-hidden="true"` plus disable pointer events.

The native `<dialog>` element (with `showModal()`) gives you focus trap and Escape handling for free. Prefer it.

## Custom combobox / listbox

Prefer `<select>`. If a custom widget is required (e.g., for autocomplete):

```html
<label for="fruit">Choose a fruit</label>
<input role="combobox" id="fruit"
       aria-expanded="false"
       aria-autocomplete="list"
       aria-controls="fruit-listbox" />
<ul id="fruit-listbox" role="listbox" hidden>
  <li role="option" id="fruit-1">Apple</li>
  <li role="option" id="fruit-2">Banana</li>
</ul>
```

Required behaviors:
- Down arrow opens the listbox; sets `aria-expanded="true"`.
- Arrow up/down moves through options; either move DOM focus or use `aria-activedescendant` on the input pointing at the current option.
- Enter selects; Escape closes; Home/End jump to first/last.
- Type-ahead: typing letters jumps to matching options.

## Tabs

```html
<div role="tablist" aria-label="Account settings">
  <button role="tab" aria-selected="true"  aria-controls="p1" id="t1">Profile</button>
  <button role="tab" aria-selected="false" aria-controls="p2" id="t2" tabindex="-1">Billing</button>
</div>
<div role="tabpanel" id="p1" aria-labelledby="t1">...</div>
<div role="tabpanel" id="p2" aria-labelledby="t2" hidden>...</div>
```

Required behaviors:
- Only the active tab has `tabindex="0"`; others have `tabindex="-1"`.
- Left/Right arrow moves between tabs (or Up/Down for vertical tab strips).
- Tab key moves *out* of the tab strip into the panel, not between tabs.
- Home/End jump to first/last tab.

## Accordion

```html
<h3>
  <button aria-expanded="false" aria-controls="section-1">
    Shipping information
  </button>
</h3>
<div id="section-1" hidden>...</div>
```

Required behaviors:
- Enter or Space toggles `aria-expanded` and shows/hides the panel.
- The button is inside a heading of the appropriate level so screen-reader users can navigate by heading.

## Toast / notification

```html
<!-- Polite: waits for the user to finish speaking -->
<div role="status" aria-live="polite">Saved.</div>

<!-- Assertive: interrupts -->
<div role="alert">Payment failed. Please check your card.</div>
```

Required behaviors:
- The live region exists in the DOM before the content is inserted  -  screen readers only announce changes to already-present live regions.
- Don't use assertive for routine confirmations; save it for errors and critical alerts.
- Auto-dismiss must be pausable (SC 2.2.1). Toasts that vanish after 3 seconds fail for slow readers.

## Icon-only button

```html
<button type="button" aria-label="Close dialog">
  <svg aria-hidden="true" focusable="false"><!-- × icon --></svg>
</button>
```

- `aria-label` supplies the accessible name.
- The SVG inside is decorative  -  hide it from assistive tech with `aria-hidden` and remove focus with `focusable="false"`.

## Form validation

```html
<label for="email">Email</label>
<input id="email" type="email"
       aria-invalid="true"
       aria-describedby="email-error" required />
<p id="email-error" role="status">Please enter a valid email address.</p>
```

- On error: set `aria-invalid="true"` and point `aria-describedby` at the message.
- A `role="status"` (polite) message announces the change without stealing focus.
- Provide a summary at the top of the form linking to each failing field  -  huge quality-of-life win for keyboard users.

## Further reading

- W3C ARIA APG (canonical): <https://www.w3.org/WAI/ARIA/apg/patterns/>
- WebAIM's ARIA article: <https://webaim.org/techniques/aria/>
- Adrian Roselli's testing notes: <https://adrianroselli.com/>
