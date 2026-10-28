---
name: website-accessibility
description: Audit websites for accessibility barriers (WCAG 2.2, ARIA, color contrast, keyboard navigation, screen-reader compatibility, alt text, focus order, semantic HTML) and propose specific, prioritized fixes with WCAG citations. Trigger this skill whenever the user mentions accessibility, a11y, WCAG, ARIA, screen readers, alt text, color contrast, keyboard-only navigation, focus, or asks whether a site is usable by people with disabilities. Also trigger when the user asks to make a component, page, or design accessible, wants to know if their site would pass an audit, is preparing for a compliance review, has received a lawsuit or complaint about accessibility, or asks how a specific WCAG success criterion works.
---

# Website Accessibility

Audit websites for accessibility barriers and propose concrete fixes, grounded in WCAG 2.2 and best-practice heuristics. This skill covers the full workflow: intake, automated scan, manual review of what automation cannot see, prioritized report, and per-issue fix suggestions.

## When to use this skill

- **Audit an existing site or page**  -  "Is my site accessible?", "Run a WCAG check on example.com", "How does my homepage do for screen-reader users?"
- **Fix a specific issue**  -  "How do I make this modal keyboard-accessible?", "Is `<div onClick>` OK if I add role='button'?"
- **Design new components accessibly**  -  "I'm building a custom dropdown, what do I need to think about?"
- **Explain a WCAG criterion**  -  "What does SC 2.4.7 mean?", "Why is 4.5:1 contrast the threshold?"
- **Prepare for compliance / legal review**  -  Section 508, ADA, EN 301 549, AODA, European Accessibility Act

## Guiding principles

**Automation catches roughly 30-40% of accessibility issues.** Studies from Deque and WebAIM consistently find that automated scanners can only detect the objectively verifiable subset of failures  -  missing alt attributes, broken form labels, contrast math. The remainder  -  meaningful alt text, sensible reading order, logical focus flow, error messages that a screen reader can hear  -  requires human judgment. A rigorous audit therefore combines both.

**Cite the specific WCAG success criterion for every issue.** "This button isn't accessible" is a complaint. "This button violates SC 4.1.2 Name, Role, Value because it lacks a discernible name for assistive tech" is an actionable finding. Every reported issue in the output must include a WCAG SC number and level (A, AA, AAA).

**Prioritize by user impact, not by rule severity.** A missing skip link is technically level A but rarely blocks anyone. A keyboard trap on a login form blocks 100% of keyboard users from ever reaching the site. Order findings by *how many users are actually blocked, and how badly*.

## Workflow

### 1. Intake  -  establish what and why

Confirm three things before scanning:

1. **What to audit**: a live URL, a local HTML file, a rendered screenshot, or a specific component/snippet. If the user says "my site," ask for the URL.
2. **The audit's purpose**: legal/compliance, remediation planning, design review, or learning. This shapes tone and depth.
3. **The compliance target**: WCAG 2.2 AA is the modern default (ADA, EN 301 549, AODA all now reference it). Only go to AAA if the user is a public-sector or accessibility-forward org and asks for it.

### 2. Automated scan

For live URLs, drive a browser to load the page and inject axe-core  -  the same library that powers Google Lighthouse's accessibility audit and the Deque axe DevTools extension. The injection pattern (runs in any browser JavaScript context: DevTools console, Puppeteer, Playwright, or a similar automation tool):

```javascript
await import('https://cdn.jsdelivr.net/npm/axe-core@4/axe.min.js');
const results = await window.axe.run();
console.log(JSON.stringify({
  violations: results.violations.map(v => ({
    id: v.id, impact: v.impact, help: v.help, helpUrl: v.helpUrl,
    wcag: v.tags.filter(t => t.startsWith('wcag')),
    nodes: v.nodes.slice(0, 5).map(n => ({ target: n.target, html: n.html.slice(0, 200) }))
  })),
  passCount: results.passes.length,
  incompleteCount: results.incomplete.length,
}, null, 2));
```

If a live browser is unavailable, fetch the raw HTML and run the structural checks in the Manual review section against the source. Note this caveat in the report  -  raw HTML analysis misses JavaScript-rendered content and dynamic ARIA.

For local files, ask the user to open the file in a browser you can drive, or paste the HTML.

**Do not stop at the automated results.** Automated findings are a starting point, not a full audit. The next section is where the real value is.

### 3. Manual review  -  the ~60% axe cannot see

Work through each area below. Note issues with the exact selector and offending HTML snippet, plus a WCAG SC citation.

**Perceivable**
- **Alt text quality.** Every meaningful image has descriptive alt text; decorative images use `alt=""` or `role="presentation"`. Complex images (charts, infographics) have a long description nearby or via `aria-describedby`. **SC 1.1.1**
- **Meaningful visual sequence.** Reading order in the DOM matches visual reading order  -  a screen reader announces content in the order it appears in the HTML, not in the order it's laid out on screen. **SC 1.3.2**
- **Info not conveyed by color alone.** Errors marked only in red, "required" fields distinguished only by red asterisk color, links distinguishable from surrounding text only by color. Add a symbol, underline, or text label. **SC 1.4.1**
- **Text can be resized 200% without loss.** Test by zooming in the browser. Content should reflow, not be clipped. **SC 1.4.4**
- **Color contrast.** Text ≥ 4.5:1 against its background (3:1 for large text ≥ 18pt or 14pt bold). Non-text UI (icon buttons, focus indicators, form borders) ≥ 3:1. Automation catches most contrast issues, but check text over gradients and images by hand. **SC 1.4.3, 1.4.11**

**Operable**
- **Everything works with the keyboard.** Tab through the page. Every interactive element must be reachable and operable via keyboard alone, in a logical order, without traps. Try opening and closing every modal, menu, and popover with keyboard only. **SC 2.1.1, 2.1.2, 2.4.3**
- **Visible focus indicator.** When an element is focused, there is a clearly visible outline. Never suppress focus with `outline: none` without replacing it. **SC 2.4.7, 2.4.11 (2.2 new)**
- **Skip link.** A "Skip to main content" link is the first tab stop on complex pages with heavy navigation. **SC 2.4.1**
- **No auto-refresh, no time limits without warning.** Session timeouts must warn the user and offer to extend. **SC 2.2.1**
- **Motion  -  no seizure risk.** No content flashes more than 3 times per second. **SC 2.3.1**
- **Touch target size.** Tap targets ≥ 24×24 CSS pixels (WCAG 2.2 addition). **SC 2.5.8**

**Understandable**
- **Page has a `<title>` and `<html lang="...">`.** Both are essential for screen-reader orientation. **SC 2.4.2, 3.1.1**
- **Form fields have programmatic labels.** Every `<input>`, `<select>`, `<textarea>` has an associated `<label>` (via `for`/`id` or wrapping), or an `aria-label` / `aria-labelledby`. Placeholder text is not a substitute. **SC 3.3.2, 4.1.2**
- **Errors are described in text, not just visually.** Error messages appear near the field, are associated via `aria-describedby`, and are announced by screen readers. **SC 3.3.1, 3.3.3**
- **Consistent navigation and identification.** Same content appears in the same relative order across pages; icons and links used for the same purpose are labeled the same way. **SC 3.2.3, 3.2.4**

**Robust**
- **Semantic HTML first, ARIA only when needed.** Use `<button>`, `<a>`, `<nav>`, `<main>`, `<h1>`-`<h6>`, `<label>` before reaching for `role="button"` etc. ARIA is a fallback for gaps in native HTML, not a decoration. The First Rule of ARIA: don't use ARIA if a native element gives you the same behavior for free.
- **ARIA roles/states/properties are used correctly.** Widget roles must match their behavior (e.g., `role="tab"` requires arrow-key navigation and `aria-selected` management). Names/labels are present for every custom widget. **SC 4.1.2**
- **Status messages announced.** Toast notifications, inline validation, and other dynamic status use `role="status"`, `role="alert"`, or `aria-live` regions so screen-reader users hear them. **SC 4.1.3**
- **Valid HTML.** Duplicate `id` attributes and malformed nesting break assistive tech. Run the source through the W3C validator when in doubt.

**Additional layers worth checking**
- **Screen reader test.** On macOS, VoiceOver (Cmd+F5) can spot-check announcements. On Windows, NVDA is free. Recommend at least a quick walk through the primary user flow.
- **200% mobile zoom.** Verify reflow at 320 CSS pixels wide with 400% zoom. **SC 1.4.10**
- **Dark mode.** If the site advertises dark mode, run the contrast pass again in that mode.

### 4. Prioritized report

Use exactly this structure so the report is scannable and actionable:

```markdown
# Accessibility audit  -  <URL or component>
_Compliance target: WCAG 2.2 <level>_ · _Date: <YYYY-MM-DD>_ · _Method: automated (axe-core) + manual review_

## Summary
- **Overall grade**: <one line, e.g., "Fails WCAG 2.2 AA. 4 blocker-severity issues affect keyboard and screen-reader users.">
- **Blockers**: <count>  ·  **High**: <count>  ·  **Medium**: <count>  ·  **Low**: <count>
- **Estimated remediation effort**: <e.g., "small  -  most issues are contrast and label fixes">

## Blockers (fix first)

### 1. <Short title>
- **WCAG**: SC <number> <name> (Level <A/AA/AAA>)
- **Impact**: <who is affected and how badly>
- **Selector**: `<CSS selector>`
- **Offending HTML**:
  ```html
  <bad snippet>
  ```
- **Fix**:
  ```html
  <good snippet>
  ```
- **Why this fix works**: <one paragraph, tying the change back to the SC>

## High-severity issues
<same structure, repeat>

## Medium-severity issues
<same structure, repeat>

## Low-severity issues
<same structure, repeat>

## Passed checks (spot-check confirmation)
- <bulleted list of things you deliberately checked and found compliant  -  useful for demonstrating audit thoroughness>

## What automation could not verify
- <list of areas that need human review or user testing  -  e.g., "cognitive load of error messages", "screen-reader announcement order in the checkout flow">

## Recommended next steps
1. <priority-ordered actions>
```

Severity mapping:
- **Blocker**: Prevents a group of users from completing a primary task (keyboard trap, no focus indicator, unlabeled critical form field).
- **High**: Significantly degrades the experience but has a workaround (poor contrast on body text, missing headings hierarchy).
- **Medium**: Real WCAG violation, moderate impact (empty alt on informative image, `<div onclick>` with `role="button"` but no keyboard handler).
- **Low**: Best-practice miss (redundant ARIA, missing skip link on a short page).

### 5. Component-authoring mode

If the user is *building* rather than auditing, switch to a checklist for the component in question. The most-frequently-built accessible components and their non-negotiables:

- **Modal / dialog**  -  `role="dialog"` with `aria-modal="true"` and `aria-labelledby`; focus moves into the dialog on open, trapped inside while open, returned to the trigger on close; Escape closes; background is `aria-hidden` or `inert`. The native `<dialog>` element with `showModal()` gives you focus trap and Escape handling for free  -  prefer it.
- **Custom dropdown / listbox**  -  Prefer native `<select>`. If custom is required, use `role="combobox"` or `role="listbox"` with arrow-key navigation, Home/End, type-ahead, Escape to close, and `aria-activedescendant` or DOM focus for the highlighted option.
- **Tabs**  -  `role="tablist"` container, `role="tab"` triggers with `aria-selected`, `role="tabpanel"` panels. Left/Right arrows move between tabs; tab key moves *out* of the tab strip, not between tabs.
- **Accordion**  -  Prefer nested `<button aria-expanded>` inside a heading (`<h3>` etc.) for the trigger, with the panel as a sibling.
- **Toast / notification**  -  `role="status"` for polite, `role="alert"` for assertive. Auto-dismiss must be pausable (SC 2.2.1).
- **Form validation**  -  Errors associated with fields via `aria-describedby`, summary at top of form linked to fields, `aria-invalid="true"` on failing fields.
- **Icon-only buttons**  -  Every one needs an accessible name via `aria-label` or visually-hidden text.

For each component, produce the semantic HTML skeleton plus the JavaScript behaviors (focus management, keyboard handling) required. See `references/aria_authoring_patterns.md` for the full canonical patterns.

### 6. Explaining a WCAG criterion

When the user asks "what does SC X.Y.Z mean?", answer with:
1. The plain-English intent of the criterion.
2. Who it protects and how.
3. One example that passes and one that fails.
4. A link to the W3C Understanding page: `https://www.w3.org/WAI/WCAG22/Understanding/<slug>`

See `references/wcag_pour.md` for a deeper walk-through of each WCAG principle (Perceivable, Operable, Understandable, Robust) organized by which user group each protects.

## Quick reference  -  the WCAG 2.2 success criteria most commonly cited in audits

| SC | Name | Level | What it means in one sentence |
|----|------|-------|--------------------------------|
| 1.1.1 | Non-text Content | A | Every non-text piece has a text alternative that conveys the same information. |
| 1.3.1 | Info and Relationships | A | Structure conveyed visually (headings, lists, tables) is also conveyed in the markup. |
| 1.3.2 | Meaningful Sequence | A | Reading order is programmatically determinable and matches visual order. |
| 1.4.1 | Use of Color | A | Color is never the only way to convey meaning. |
| 1.4.3 | Contrast (Minimum) | AA | Text meets 4.5:1 (3:1 for large text). |
| 1.4.4 | Resize Text | AA | Text can be resized to 200% without loss of content or function. |
| 1.4.10 | Reflow | AA | Content reflows to 320 CSS pixels wide without two-dimensional scrolling. |
| 1.4.11 | Non-text Contrast | AA | UI components and graphical objects meet 3:1 contrast. |
| 2.1.1 | Keyboard | A | All functionality is available from a keyboard. |
| 2.1.2 | No Keyboard Trap | A | Keyboard focus can leave any component it enters. |
| 2.2.1 | Timing Adjustable | A | Time limits can be turned off, adjusted, or extended. |
| 2.3.1 | Three Flashes | A | No content flashes more than three times per second. |
| 2.4.1 | Bypass Blocks | A | A mechanism (skip link, landmarks) lets users skip repeated blocks. |
| 2.4.2 | Page Titled | A | Every page has a descriptive `<title>`. |
| 2.4.3 | Focus Order | A | Focus moves in an order that preserves meaning. |
| 2.4.6 | Headings and Labels | AA | Headings and labels describe their topic or purpose. |
| 2.4.7 | Focus Visible | AA | Keyboard focus is always visually indicated. |
| 2.4.11 | Focus Not Obscured (Min.) | AA (2.2 new) | Focused element is not entirely hidden by sticky headers/dialogs. |
| 2.5.8 | Target Size (Min.) | AA (2.2 new) | Interactive targets are ≥ 24×24 CSS pixels. |
| 3.1.1 | Language of Page | A | `<html lang="...">` declares the page language. |
| 3.2.3 | Consistent Navigation | AA | Navigation blocks appear in the same relative order across pages. |
| 3.3.1 | Error Identification | A | Errors are identified and described in text. |
| 3.3.2 | Labels or Instructions | A | Form inputs have labels or instructions. |
| 3.3.3 | Error Suggestion | AA | When errors are detected, suggest a correction if possible. |
| 4.1.2 | Name, Role, Value | A | All custom UI has a programmatic name, role, and (where relevant) value/state. |
| 4.1.3 | Status Messages | AA | Status updates are announced by assistive tech without a focus change. |

Full catalog: <https://www.w3.org/WAI/WCAG22/quickref/>

## Common fixes cheat-sheet

| Symptom | Fix |
|---------|-----|
| Icon-only button silent to screen readers | `<button aria-label="Close">×</button>` |
| Placeholder used as the only label | Add a real `<label for="id">` or `aria-label` |
| `<div onclick="...">` acts as a button | Replace with `<button>`  -  inherits keyboard, focus, role for free |
| Modal doesn't trap focus | Use the `<dialog>` element with `showModal()` (or manage focus with JS) plus `aria-modal="true"` |
| Focus outline removed for aesthetics | Never `outline: none` without a visible replacement  -  use `:focus-visible { outline: 2px solid <accent>; outline-offset: 2px; }` |
| Toast notification not announced | Wrap in `<div role="status">` for polite or `role="alert"` for assertive |
| Body text 3.8:1 on gray-on-gray | Darken the text or lighten the background until ≥ 4.5:1 (small text) or 3:1 (18pt+) |
| Nested `<a>` inside `<button>` | Split into two elements  -  HTML forbids interactive nesting |
| Custom dropdown ignored by keyboard | Prefer native `<select>`; if custom, implement full ARIA combobox pattern with arrow keys |
| `<h1>` used for a small subheading | Match heading level to document outline; use CSS to restyle |

## Tone and delivery

- Be concrete. Every issue names a selector, a fix, and the SC it violates.
- Be honest about limits. If a live browser isn't available and you're auditing raw HTML, say so: dynamic ARIA, JS-rendered content, and focus behavior can't be verified.
- Be constructive. Frame findings as "here's how to make this work for more of your users," not "you failed."
- When the user is new to accessibility, spend an extra sentence on *who* is protected by a given criterion  -  a coefficient of empathy that keeps the audit from feeling like a checklist.

## Further reading

- W3C WCAG 2.2 Understanding docs (canonical): <https://www.w3.org/WAI/WCAG22/Understanding/>
- W3C ARIA Authoring Practices Guide (canonical widget patterns): <https://www.w3.org/WAI/ARIA/apg/>
- WebAIM's Million report (annual scan of top 1M home pages): <https://webaim.org/projects/million/>
- Deque axe-core rules reference: <https://dequeuniversity.com/rules/axe/>
- Adrian Roselli's practical testing notes: <https://adrianroselli.com/>
