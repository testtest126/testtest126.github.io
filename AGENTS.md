# AGENTS.md

<!--
  The single source of truth for how AI agents (and humans) work in this repo.
  Model-agnostic entry point — CLAUDE.md just points here.
-->

## What this project is
`testtest126.github.io` is Yakiv Kovalskyi's personal GitHub Pages site: a
portfolio homepage (`index.html`) plus a standalone series of self-contained
explainer pages, each at its own slug — `/trust-nothing/`, `/hello-human/`,
`/tiny-prompt/`, `/access-granted/`, `/number-of-the-beast/`, `/specimen-01/`,
`/swarm/`, and the practical `/gcd-puzzles/` trainer, all indexed at `/notes/`.
Every page is a single self-contained `index.html` — inline CSS, inline JS
where needed, no build step, no dependencies. The explainer pages are
deliberately not linked from the homepage; they're a separate, standalone body
of work (see `/notes/`).

## Build, run, test
- **Setup:** none. Plain HTML/CSS/(vanilla) JS — no `package.json`, no bundler, no framework.
- **Build:** none — GitHub Pages serves the repo root as-is. Nothing to compile.
- **Run / preview locally:** `python3 -m http.server 8000` from the repo root, then open `http://localhost:8000/<slug>/`.
- **Test:** no automated suite. Verification is manual and browser-based:
  - Load the page and check the browser console for errors (zero tolerated).
  - Screenshot at desktop (~1280px) and mobile (~390px); confirm no horizontal overflow.
  - Exercise any interactive elements (e.g. `/swarm/`'s "Run the swarm" button, `/gcd-puzzles/`'s answer checks) and confirm they actually work, not just render.
  - After pushing, poll the Pages build and confirm the live URL returns HTTP 200 before calling anything shipped:
    `gh api repos/testtest126/testtest126.github.io/pages/builds/latest --jq '.status,.commit'`

## Conventions
- **One page = one self-contained `index.html`.** Palette, type, and layout live inline in a `<style>` block — no shared stylesheet, no CDN, no Google Fonts. A page's own assets are referenced as `/assets/...` (absolute — pages live one directory deep).
- **The explainer series shares one visual system** (the "Legents" palette): `--bg:#07090b; --ink:#e9e6df; --gold:#c9a24a; --jade:#4bbf82; --red:#c0413b;` a serif body face (Georgia / Iowan Old Style) plus mono accents (Courier New). Reuse it verbatim for new pieces in the series — don't invent a new palette per page.
- **New explainer pages cross-link.** Each page's footer links to its companion pieces and to `/notes/`; add new pages to `/notes/index.html`'s numbered entries list with a one-line description, and link back from the new page.
- **The series' ethos is "trust nothing you can't verify."** Any factual claim, decode, or quoted source in an explainer page gets independently checked before publishing — including claims made in the prompt that requested the page. Where a claim can't be independently re-verified (e.g. something sourced from a screenshot), say so plainly in a provenance note instead of presenting it as confirmed.
- **Deploy:** GitHub Pages serving `main` directly — no `gh-pages` branch, no Actions build. `.nojekyll` disables Jekyll processing. Pushing to `main` *is* the deploy. No branch protection is configured; direct pushes to `main` are normal, though PRs get used for larger changes too.
- **Commits:** short, descriptive summary line (e.g. `Add /swarm/ explainer — root → orchestrator → worker, with a real run`). No fixed prefix scheme.

## The hard rule: index.html (the homepage) is immutable
`index.html` at the repo root is Yakiv's live portfolio homepage. **Do not
modify it** — not its content, not its whitespace, not a single byte — unless
the user explicitly asks for a homepage change in so many words.

Its current known-good checksum:
```
44d63058a9cc4609936b013a2ee756bb   (MD5)
```
Verify it's unchanged before starting work and again before committing:
```
md5 index.html   # or: md5sum index.html
```
If a task doesn't strictly require touching `index.html`, don't touch it — new
work goes in a new directory/slug (`/whatever-slug/index.html`), never into the
homepage. If the checksum ever legitimately needs to change, that's a
deliberate, explicitly-requested homepage edit — call it out clearly and get
confirmation before pushing.

## Do not
- Don't modify `index.html` outside an explicit request to change the homepage (see the hard rule above).
- Don't add a build step, framework, or runtime dependency — plain HTML/CSS/JS is a deliberate constraint, not an oversight.
- Don't fabricate facts, quotes, or computed results in an explainer page — the collection's whole premise is verification; a fabricated "verified" claim undermines the entire series.
- Don't reproduce copyrighted material (comic panels, article text, images) in a new page — describe/paraphrase and link out instead, per the pattern already used in `/access-granted/` and `/hello-human/`.
- Don't leave a new page unlinked — cross-link it into `/notes/` and its nearest companion pieces' footers.
- Don't push and call it shipped without checking the Pages build actually goes green and the live URL returns 200 — a red build or a 404 is the whole point of verifying, missed.

## Layout
- `index.html` — the live homepage (see the hard rule above).
- `assets/` — project screenshots used by the homepage's project cards.
- `<slug>/index.html` — each explainer/tool page, one per directory: `trust-nothing/`, `hello-human/`, `tiny-prompt/`, `access-granted/`, `number-of-the-beast/`, `specimen-01/`, `swarm/`, `gcd-puzzles/`.
- `notes/index.html` — the index page tying the explainer series together.
- `read-nothing/` — an empty leftover directory with no `index.html`; not a live page, safe to ignore.
- `.nojekyll` — disables GitHub Pages' Jekyll processing.
- `README.md` — one-line pointer to the live site.
