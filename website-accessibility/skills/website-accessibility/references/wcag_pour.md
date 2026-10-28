# WCAG 2.2  -  the POUR principles, and who each protects

WCAG groups its 78 success criteria under four principles: **Perceivable, Operable, Understandable, Robust** (POUR). This reference walks through each principle from a user's-eye view: *who is affected when this fails*.

## Perceivable  -  the user must be able to sense the content

- **Blind users** rely on screen readers (JAWS, NVDA, VoiceOver, TalkBack). Every image, icon, chart, and video needs a text alternative that carries the same meaning. Decorative images should be marked so the screen reader skips them (`alt=""` or `role="presentation"`).
- **Low-vision users** use screen magnifiers (up to 400%) and often set custom colors or high-contrast modes. Content must reflow at 320 CSS pixels wide, and information carried by color alone (a red asterisk, an error tint) must also be carried by a symbol or text.
- **Deaf / hard-of-hearing users** need captions on all pre-recorded audio and video and transcripts for audio-only content.
- **Colorblind users** (about 8% of men, 0.5% of women globally) can't distinguish some color pairs. Never rely on color alone  -  red/green, blue/yellow, and green/brown are the common trouble spots.
- **Users with photosensitive epilepsy** can seize from more than three flashes per second in a large area (SC 2.3.1). Any moving visual pattern deserves a check.

## Operable  -  the user must be able to interact

- **Motor-impaired users** may use keyboard only, switch controls, voice control, or eye tracking. Every interactive element must be operable with the keyboard alone, in a logical order, with no traps.
- **Screen-reader users** navigate primarily via headings, landmarks, and links  -  a page with no headings is a page with no navigable structure. Skip links let them bypass repeated navigation.
- **Users with cognitive or motor differences** benefit from generous target sizes (24×24 CSS pixels minimum, WCAG 2.2 SC 2.5.8), no forced timeouts, and no rapidly-changing content.
- **All keyboard users** need a visible focus indicator. This is the single most-violated criterion in modern designs  -  designers strip `outline` for aesthetics and forget to replace it.

## Understandable  -  the user must be able to make sense of the content and UI

- **Users with cognitive disabilities, dyslexia, and non-native speakers** benefit from clear language, consistent navigation, predictable behavior, and generous error handling. Errors should be described in text (not just flagged in red), and where possible the system should suggest a correction ("Did you mean 2024?").
- **All users** benefit when the page declares its language (`<html lang="en">`)  -  assistive tech uses it to pick the right pronunciation model. A Spanish page rendered with an English pronunciation engine is unlistenable.
- **Screen-reader users** benefit from descriptive headings, labels, and link text. "Click here" is meaningless when scanned as a list of links out of context.

## Robust  -  the content must work with today's and tomorrow's assistive tech

- **All AT users** benefit when developers use semantic HTML (`<button>`, `<a>`, `<nav>`, `<main>`) first and reach for ARIA only when native HTML can't do the job. ARIA is a fallback, not a decoration  -  the First Rule of ARIA is: "Don't use ARIA if a native element does the same job."
- **When ARIA is necessary** (custom widgets, dynamic content), it must be used correctly. A `role="tab"` with no arrow-key handling is a broken promise to the screen-reader user, and screen readers will announce it as a tab that doesn't work.
- **Dynamic content**  -  toasts, inline validation, loading states  -  must be announced via `role="status"`, `role="alert"`, or `aria-live` regions, because sighted users see the change but non-sighted users don't.

## Where to go next

- W3C's official guide, organized by criterion with test procedures: <https://www.w3.org/WAI/WCAG22/Understanding/>
- Quickref for filtering by level/tag: <https://www.w3.org/WAI/WCAG22/quickref/>
- WebAIM's practical articles: <https://webaim.org/articles/>
- Deque's blog for real-world case studies: <https://www.deque.com/blog/>
