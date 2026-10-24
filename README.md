# Paul — static site

Two self-contained pages, published with GitHub Pages.

| Page | URL | What it is |
|---|---|---|
| Todo list app | https://paullaucwgmail.github.io/Paul/ | Interactive todo list (add / mark done / delete) |
| Meeting deck | https://paullaucwgmail.github.io/Paul/deck/ | Traditional-Chinese presentation built from the one-hour meeting record |

## Pages

### `index.html` — Todo list app

- **Add** — type and press Enter (or the button); blank input is rejected, 200-character cap
- **Mark done** — custom checkbox with an SVG tick; completed items get a strikethrough and fade
- **Delete** — per-row delete button, animated out
- **Extras** — live counter, "clear completed", empty state, `localStorage` persistence

Design & accessibility: design tokens (8pt spacing scale, consistent radii/shadows, 180 ms
transitions), dark/light theme following `prefers-color-scheme` with a remembered manual
toggle, real `<label>`s plus `aria-label`s, an `aria-live` status region, a visible
`:focus-visible` ring, full keyboard operation, `prefers-reduced-motion` support. All user
text is inserted with `textContent`, so pasted HTML/JS renders as plain text.

### `deck/index.html` — Meeting deck

Thirteen 16:9 slides in Traditional Chinese built strictly from
`一小時會議原始記錄.docx` (2026-07-06, one-hour meeting): cover, background, current state,
findings, supporting evidence, architecture, A/B comparison, 3-hour breakdown, governance
design, risks, opportunities, timeline, next steps.

- Navigation: previous/next buttons, ←/→, Page Up/Down, Home/End, swipe on touch
- Progress bar plus a page counter, and a slide overview (`O`) with jump-to-slide
- Speaker notes (`N`) showing the source lines behind each slide
- Fullscreen (`F`), theme toggle, print/PDF export with one slide per page
- Dark/light mode, `prefers-reduced-motion`, ARIA labels and a live region
- Every claim traces to the source document; nothing was invented

## Project layout

```
index.html                     todo app (self-contained)
deck/index.html                meeting deck (self-contained)
.github/workflows/deploy.yml   build + deploy to GitHub Pages
dist/                          build output (generated in CI, not committed)
```

## Local development

Open either HTML file in a browser — no build step and no server required.
To serve the site over HTTP instead:

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

## Deployment

Every push to `main` runs `.github/workflows/deploy.yml`: it validates both pages
(非空、含 `<html>`/`</html>`、沒有外部資源引用), copies them into `dist/`, uploads the Pages
artifact and deploys it. Manual runs are available from the Actions tab
(`workflow_dispatch`).

**One-time setup:** GitHub does not allow a workflow to switch Pages on — the Actions
`GITHUB_TOKEN` is blocked from creating a Pages site (`Resource not accessible by
integration`). Enable it once by hand:

1. Repository **Settings** → **Pages**
2. **Build and deployment** → **Source: GitHub Actions**

## Verification

- Todo app: jsdom smoke test, 27 assertions (add / reject blank / mark done and undo /
  counter and button states / `localStorage` write and reload / delete with animation /
  clear-completed / HTML-injection safety / accessibility attributes).
- Deck: jsdom test, 48 assertions (initial state, button and keyboard navigation, overview
  jump, speaker-notes panel, theme persistence, print, non-throwing fullscreen fallback,
  unique slide titles, key facts present in the body).

## Licence

MIT
