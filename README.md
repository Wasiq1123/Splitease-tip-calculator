# SplitEase — Tip Calculator & Bill Splitter

A single-screen, live-updating tip calculator and bill splitter. No frameworks, no build step, no dependencies. Just open the file.

## How to run locally

**Option 1 — Open directly (easiest)**
```
open index.html
```
Or double-click `index.html` in your file manager. Works in any modern browser without a server.

**Option 2 — Local dev server (if you prefer)**
```bash
npx serve .
# then open http://localhost:3000
```
Or with Python:
```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

No `npm install` needed. No build step. No environment variables.

## Stack

Vanilla HTML, CSS, and JavaScript — no frameworks, no bundler.

## Project structure

```
tip-calculator/
├── index.html      # Everything: markup, styles, logic
└── README.md
└── ANSWERS.md
```

## Features

- Live calculation as you type — no submit button
- Tip presets (10% / 15% / 20%) + custom percentage input
- People stepper with +/− buttons
- Inline validation with graceful error/clear behaviour
- Rounding policy: round UP to nearest paisa so the group never underpays
- Full keyboard navigation, ARIA labels, focus states
- Responsive: works on 360px phones and 1440px desktops
- Reset button returns app to clean state
