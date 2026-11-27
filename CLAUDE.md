# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The GitHub Pages user site for `vudinhhuyit.github.io`: a personal portfolio homepage. It's plain static HTML with no build step, package manager, linter or tests. Pushing to `main` deploys it.

- `index.html`: the whole site. CSS sits in one `<style>` block, JS in one `<script>` block, and nothing is loaded from outside except Google Fonts (Be Vietnam Pro).
- `assets/icons/*.png`: app icons, 256×256 PNG.
- `app-ads.txt`: AdMob publisher verification for the iOS apps listed on the page. It has to stay at the domain root, so don't touch it unless asked.

Local preview: `python3 -m http.server 8000` from the repo root. Root-absolute links such as `/Talk-Wallet-Support/`, `/Doc2Markdown/` and `/Doc2Markdown/app/viewer.html` point to *other* repos' project Pages on the same domain. They return 404 locally, and that's expected. A few icons are hotlinked from those sites too (`https://vudinhhuyit.github.io/Doc2Markdown/...`).

## Bilingual text (VI/EN)

Vietnamese is the source text and is written directly in the HTML. English lives only in the `EN` object in the `<script>` at the bottom of `index.html`.

- Every translatable element has `data-i18n="key"` (swapped through `innerHTML`, so values may contain markup like `<span class="hl">`). Image alt text uses `data-i18n-alt` and aria labels use `data-i18n-aria`. `meta.title` and `meta.desc` are swapped in code.
- When the page loads, the script saves each element's Vietnamese content. Switching back to VI restores those saved values.
- **Every `data-i18n*` key needs a matching `EN` entry.** A missing key makes the English view show `undefined`. Keys may be reused; for example, `tw.download` is the label on every App Store button.
- Keys are prefixed per project (`tw.`, `rv1.`, `sd2.`, `sdx.`, `capy.`, `hub.` …).
- The starting language comes from `?lang=en|vi` in the URL, then `localStorage.lang`, then `navigator.language`.
- Vietnamese copy is casual and first-person ("mình").

## Project cards

Projects are `<article class="project reveal">` cards grouped under `.group-title` headings: iOS apps, Games, Coming soon and Productivity tools.

- Each `.group-title` has a hardcoded `<span class="count">N</span>`. Update it whenever you add or remove a card in that group.
- Status tags: `.tag.live` (green, "● Đã có trên App Store") and `.tag.soon` ("◔ Sắp ra mắt"). When an app goes live: change the tag, add the App Store `.links a.go` button, and move the card from "Sắp ra mắt" to "Game".
- `.project.wide` spans the full row in the 2-column `.grid`. `.grid.three` is the 3-column games layout.
- Hidden projects (Wheely, Snoring, Red Ball, Jira Helpers) are kept in HTML comments marked `Tạm ẩn`. They have no `data-i18n` keys, so restoring one means adding keys and `EN` entries.

## Hero icon ring

The `.float-icon` elements around the "H" avatar sit evenly on a circle. Each one's position is set with inline `--x` and `--y` percentages. For N icons, icon i (0-based) goes at:

```
θ = (i + 0.5) · 360° / N      (clockwise from top)
x = 50 + 48·sin θ   y = 50 − 48·cos θ
```

`animation-delay` values are staggered across the 5 s `bob` cycle. Adding or removing an icon means recomputing every position, not just adding one.

## Styling

The site is dark theme only. All colors are CSS custom properties in `:root`. The accent (`--accent`) is a very light lavender with dark `--accent-ink` text on top of it. `prefers-reduced-motion` turns off all animations and transitions. The responsive breakpoints are 860px (single column) and 560px (nav links without `.keep` are hidden).
