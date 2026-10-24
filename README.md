# Paul — Todo List Web App

A single-file, dependency-free todo list that runs entirely in the browser.

**Live:** https://paullaucwgmail.github.io/Paul/

## Features

- **Add** — type and press Enter (or the button); blank input is rejected, 200-character cap
- **Mark done** — custom checkbox with an SVG tick; completed items get a strikethrough and fade
- **Delete** — per-row delete button, animated out
- **Extras** — live counter, "clear completed", empty state, `localStorage` persistence

## Design & accessibility

- Design tokens (CSS custom properties): 8pt spacing scale, consistent radii and shadows, 180 ms transitions
- Dark / light theme — follows `prefers-color-scheme`, with a manual toggle that is remembered
- Semantics: `<main>`, `<ul>`, real `<label>`s; `aria-label` on the checkbox and delete button;
  `aria-live="polite"` status region; visible `:focus-visible` ring; fully keyboard operable
- Responsive down to small phones; honours `prefers-reduced-motion`
- All user text is inserted with `textContent`, so pasted HTML/JS renders as plain text

## Project layout

```
index.html                     the whole app (HTML + CSS + JS, no dependencies)
.github/workflows/deploy.yml   build + deploy to GitHub Pages
dist/                          build output (generated in CI, not committed)
```

## Local development

Open `index.html` in any browser — there is no build step and no server required.
To serve it over HTTP instead:

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

## Deployment

Every push to `main` runs the workflow in `.github/workflows/deploy.yml`:
it validates `index.html`, copies it to `dist/`, uploads the Pages artifact and deploys it.
Manual runs are available from the Actions tab (`workflow_dispatch`).

## Verification

The app's behaviour is covered by a jsdom smoke test (27 assertions): add / reject blank /
mark done and undo / counter and button states / `localStorage` write and reload /
delete with animation / clear-completed / HTML-injection safety / accessibility attributes.

## Licence

MIT
