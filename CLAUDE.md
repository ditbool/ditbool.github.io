# CLAUDE.md

This file provides guidance for AI assistants working in this repository.

## Project Overview

This is a **static single-page conference event website** hosted on GitHub Pages at `ditbool.github.io`. The site promotes the **2025 과학기술문화 컨퍼런스** (2025 Science & Technology Culture Conference). It uses no build tools — the entire site is a single `index.html` file.

## Repository Structure

```
ditbool.github.io/
├── index.html        # Entire website (HTML + embedded CSS + embedded JS)
├── images/
│   ├── .gitkeep
│   └── free-animated-icon-info-15578675.gif
└── CLAUDE.md         # This file
```

## Tech Stack

| Layer     | Technology |
|-----------|-----------|
| Markup    | HTML5 (semantic, `lang="ko"`) |
| Styling   | CSS3 embedded in `<style>` within `<head>` |
| Scripting | Vanilla ES6 JavaScript (ES module, embedded in `<script type="module">`) |
| 3D FX     | [`threejs-components@0.0.19`](https://cdn.jsdelivr.net/npm/threejs-components@0.0.19/build/cursors/tubes1.min.js) via CDN |
| Fonts     | Google Fonts — Montserrat (weights 300, 400, 500, 700) |
| Hosting   | GitHub Pages (auto-deploys from `master`) |

**No package manager, no bundler, no build step.** Do not add them unless explicitly requested.

## Development Workflow

### Running locally
Open `index.html` directly in a browser. No server required; all resources load from CDN.

### Editing
- All HTML structure, CSS styles, and JavaScript logic live in `index.html`.
- There is no source/build separation — edits to `index.html` are production edits.

### Deployment
Push to the `master` branch. GitHub Pages automatically serves the updated file.

### Testing
No automated tests exist. Verify changes manually in a browser (desktop and mobile viewports).

## Code Conventions

### HTML
- Language attribute: `lang="ko"` (Korean-language content)
- Root wrapper: `<div id="app">`
- Fixed 3D canvas: `<canvas id="canvas">` — always the first child of `#app`, `position: fixed`, `z-index: 0`
- Sections use semantic `<section>` tags, each wrapping a `<div class="container">`

### CSS (embedded `<style>`)
- All styles are in one `<style>` block in `<head>` — no external stylesheet
- **Glassmorphism** pattern: `background: rgba(255,255,255,0.1)` + `backdrop-filter: blur(10px)` + `border-radius`
- **Layout**: CSS Grid for cards (`repeat(3, 1fr)`), Flexbox for centering
- **Single breakpoint**: `@media (max-width: 768px)` collapses grids to `1fr` and scales down font sizes
- **z-index layering**: canvas `z-index: 0`, `.hero` and `.content` at `z-index: 1`
- Colors: all text is `white`; muted text uses `rgba(255,255,255,0.7)`
- Section font scale: h1 `80px`, h2 `60px`, `.section-title` `60px` (→ `36px` on mobile)

### JavaScript (embedded `<script type="module">`)
- Uses ES module syntax (`import ... from "..."`)
- Imports `TubesCursor` from CDN and initializes it on `#canvas`
- Click handler on `document.body` randomises tube and light colors via `app.tubes.setColors()` / `app.tubes.setLightsColors()`
- Color helper: `randomColors(count)` returns an array of random hex strings

## Page Sections (top → bottom)

| Section | Class/Element | Description |
|---------|--------------|-------------|
| Background | `#canvas` | Fixed 3D animated tubes (Three.js) |
| Hero | `.hero` | Full-viewport title block |
| Event Info | `section > .container` | 3-column info grid (date, location, fee) |
| Program | `section > .container` | Timed agenda list (`.program-item`) |
| Speakers | `section > .container` | 3-column speaker cards (`.speaker-card`) |
| CTA | `section[style]` | Registration button (`.cta-button`) |
| Footer | `footer` | Organiser info, copyright |

## Placeholder Content (needs real data)

The following fields contain placeholder values and must be updated before launch:

- **Date**: `2025. 00. 00` → actual event date
- **Location**: `서울 OO홀` → actual venue name
- **Speaker names**: `홍길동`, `김철수`, `이영희` → real speaker names
- **Speaker titles**: `OO대학교 교수`, etc. → real affiliations
- **Speaker photos**: `연사 사진` placeholder divs → actual `<img>` elements in `images/`
- **CTA link**: `href="#"` on the registration button → real registration URL
- **Contact email**: `contact@scienceconf2025.org` → verified address
- **Contact phone**: `02-1234-5678` → verified number

## Key Constraints

- **No build tooling** — do not introduce npm, webpack, Vite, etc. unless explicitly asked.
- **Single-file architecture** — keep all code in `index.html` unless the user requests splitting files.
- **No frameworks** — no React, Vue, etc.; this is intentional vanilla HTML/CSS/JS.
- **CDN-only dependencies** — all external libraries must be loaded from CDN, not installed locally.
- **Korean content** — the site serves a Korean audience; preserve `lang="ko"` and Korean text conventions.
- **Accessibility** — speaker photo placeholders lack `alt` text; add `alt` attributes when replacing them with real images.

## Git Workflow

- Default branch for content: `master` (GitHub Pages source)
- Feature/AI branches follow the pattern: `claude/<session-id>`
- Commit directly to `master` for simple content updates; use feature branches for structural changes
