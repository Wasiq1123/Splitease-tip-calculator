# ANSWERS.md

---

## 1. How to run

No installation required. The entire app is one HTML file with zero external dependencies installed locally.

**Quickest way:**
```
open index.html
```
Double-clicking `index.html` works in Chrome, Firefox, Edge, and Safari.

**If you want a proper dev server:**
```bash
npx serve .
# visit http://localhost:3000
```

Or:
```bash
python3 -m http.server 8080
# visit http://localhost:8080
```

No `npm install`, no `.env`, no build step needed at all. The Google Fonts import is the only network call, and the app degrades gracefully to system sans-serif if that request fails.

---

## 2. Stack & design choices

**Why vanilla HTML/CSS/JS?**

Honestly, for a task like this, reaching for React or Vue would have been over-engineering it. There's no shared state across components, no routing, no async data. The whole thing is: three inputs → one formula → four outputs. Vanilla JS handles that in under 150 lines. I also didn't want a `node_modules` folder or a build command standing between someone opening the file and seeing it work — the assessors can just double-click it.

**Design decision 1 — Sticky dark result panel**

I put the results in a separate dark card on the right (or below on mobile) that sticks to the viewport as you scroll. My thinking was: when you're splitting a bill in real life, the one number you keep glancing at is "how much do I owe?" I wanted that number to always be visible without the user having to scroll down to see if their input changed anything. The large monospace per-person amount at the bottom of that panel takes the most visual space because that's the number people actually care about — the sub-totals are context, not the destination.

**Design decision 2 — Preset tip buttons + custom input as a toggle, not two separate fields**

I chose to make the custom percentage field hidden by default and only appear when "Custom" is pressed. If I'd shown a text field next to the buttons, users would have to figure out which one "wins." There'd also be a conflict: what happens if you type 25% in the custom field but 20% is still highlighted? That ambiguity causes bugs. By making the custom button deactivate the presets and vice versa, the active state is always clear — you can see at a glance which mode is live. The active preset gets filled with the accent colour so there's no guessing.

---

## 3. Responsive & accessibility

**Responsive behaviour**

On a 360px phone, the two-column layout collapses to a single column — inputs stack on top, results sit below. I used `grid-template-columns: 1fr 1fr` with a `@media (max-width: 620px)` breakpoint that switches it to `1fr`. Font sizes use `clamp()` for the heading so it doesn't overflow on narrow screens. The stepper buttons are 48px tall — large enough to tap comfortably without accidentally hitting the adjacent field. The result panel loses its `position: sticky` behaviour on mobile naturally because it's below the inputs, not beside them.

On a 1440px desktop, the layout sits in a `max-width: 780px` centred container. The two columns breathe without stretching uncomfortably wide.

**Accessibility I handled**

Every input has an `aria-describedby` pointing to its error message element, and those error elements have `role="alert"` and `aria-live="polite"` — so screen readers announce validation errors as they appear without interrupting the user mid-sentence. The tip preset buttons use `aria-pressed` to communicate the active/inactive toggle state. The stepper's `+` and `−` buttons have explicit `aria-label` attributes ("Increase people", "Decrease people") because the visual symbols alone aren't enough. The result region has `role="region"` with an `aria-label`, and the per-person amount has `aria-live="polite"` so screen readers read it out when the number updates.

Tab order follows a natural reading order: bill → tip buttons → custom tip (only when visible) → people stepper → reset. Pressing Enter in a text field moves focus to the next logical field.

**Accessibility I knowingly skipped**

I didn't add a high-contrast mode toggle or test specifically against WCAG 2.1 AA contrast ratios for every colour combination — particularly the lighter `ink3` labels against the white card backgrounds and the `rgba(255,255,255,0.55)` labels inside the dark result card. Those likely fail the 4.5:1 ratio for normal-weight text. With another day I'd run the whole palette through a contrast checker and either darken the muted text or increase the font weight so smaller text passes at the 3:1 large-text threshold. I didn't do it during this submission because I prioritised the interaction correctness first.

---

## 4. AI usage

**What I used AI for:**

I used Claude (claude.ai) for two things:

1. **Initial HTML skeleton and CSS variable structure** — I asked it to generate a starter layout with a two-column grid, a dark result panel, and some CSS custom properties for theming. It gave me a reasonable bones but with a very generic purple-on-white colour scheme and Inter as the font.

2. **Rounding policy logic** — I asked it to show me JavaScript for rounding per-person amounts. It gave me `Math.round(perPerson * 100) / 100`.

**What I changed and why:**

For the rounding: the AI gave me standard banker's rounding (`Math.round`), which rounds halves to nearest even. That's fine for accounting software but not for a dinner table. If the per-person share is Rs 83.335, rounding to Rs 83.33 means the group collectively pays less than the actual total — that difference has to come from somewhere. I changed it to `Math.ceil(perRaw * 100) / 100` so every person always rounds up. The group might collectively overpay by a paisa or two, but nobody underpays and nobody has to do the mental arithmetic of "who pays the extra Rs 0.02." That felt like the right call for the use case.

For the styling: I scrapped the AI's colour scheme entirely. Purple gradients on white is the most AI-generated-looking output possible. I went with a warm off-white background (`#f5f3ee`), a burnt orange accent (`#c8440e`), and a near-black dark card for the results. I also switched from Inter to Syne (for headings/labels) and DM Mono (for the numbers) — monospace numbers prevent the layout from jumping when digits change width, and Syne has a distinct character that makes it feel designed rather than default.

**Honest gap:**

The custom tip input doesn't handle the edge case where someone types something like `1e5` (scientific notation) — `parseFloat('1e5')` returns 100000, which passes my `<= 100` check and blows up the calculation. I added a paste-sanitisation handler for obvious garbage but didn't write a proper input parser that rejects scientific notation specifically. With another day I'd replace the raw `parseFloat` with a stricter regex parse that only accepts `\d+(\.\d{0,2})?` format before handing off to the number conversion.
