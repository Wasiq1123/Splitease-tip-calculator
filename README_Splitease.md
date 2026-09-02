# SplitEase — Tip Calculator & Bill Splitter

A single-page, live-updating tip calculator and bill splitter. No frameworks, no build step, no dependencies — open the HTML file and it works.

## Run locally

**Option 1 — open directly**
```bash
open index.html
```
Or double-click `index.html`. Works in any modern browser, no server required.

**Option 2 — local dev server**
```bash
npx serve .
# then open http://localhost:3000
```
or
```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

No `npm install`, no build step, no environment variables.

## Stack

Vanilla HTML, CSS, and JavaScript.

## Features

- Live calculation as you type, no submit button
- Tip presets (10% / 15% / 20%) plus custom percentage input
- People-count stepper with +/− controls
- Inline validation with error/clear states
- Rounding policy: rounds up to the nearest paisa so the group never underpays as a whole
- Keyboard navigation, ARIA labels, focus states
- Responsive from 360px phones to 1440px desktops
- Reset button returns the app to its initial state

## Known limitations

- Not tested against WCAG 2.1 AA contrast ratios for all color pairs — some muted label text likely falls short of the 4.5:1 threshold.
- The custom tip input accepts scientific notation (e.g. `1e5`), which passes the numeric range check and produces an invalid result. A stricter input parser is needed to reject non-decimal formats.

See `ANSWERS.md` for the full design rationale, accessibility notes, and AI-usage disclosure.

## Project structure

```
Splitease-tip-calculator/
├── index.html
├── README.md
└── ANSWERS.md
```
