# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page marketing website for Redemption TenFold, an award-winning tattoo studio collective in Menominee, Michigan. The entire site is a static HTML file with no build process, package manager, dependencies, or tests.

## Repository structure

- `index.html` — the entire site: all CSS (in a `<style>` block) and all JS (in a `<script>` block at the bottom) live in this one file. There is no separate CSS/JS tooling.
- `images/` — artist headshots, portfolio photos, and a studio video referenced directly by relative path (e.g. `images/adam-zimmerman.jpg`).

There is no `package.json`, build config, linter, or test suite. Everything is plain HTML/CSS/JS served as-is.

## Development workflow

There is no build/compile/lint/test step. To work on the site:
- Edit `index.html` directly.
- Preview by opening the file in a browser, or serve it locally, e.g. `python3 -m http.server` from the repo root and visit `http://localhost:8000`.
- Verify changes visually in a browser — there is no automated test suite, so manual visual/functional checks (scroll reveal, lightbox, mobile menu, FAQ accordion) are the only verification available.

## Architecture / page structure

`index.html` is organized as stacked `<section>`s, top to bottom: nav → hero → marquee → philosophy → about → artists → studio video → reviews → FAQ → aftercare → location/map → booking → footer. CSS custom properties at `:root` (`--gold`, `--black`, `--text`, etc.) define the dark/gold color theme used throughout.

Key patterns to follow when editing:

- **Image fallbacks**: every `<img>` has an `onerror` handler that swaps in an Unsplash stock photo if the local file in `images/` is missing (e.g. `onerror="this.src='https://images.unsplash.com/...'"`). Preserve this pattern when adding new images so the site degrades gracefully if an asset is absent.
- **Artist cards** (`.artist-card`, under `#artists`): each artist is a 3-zone block — identity bar (headshot, name, book button), bio section (2-column text), and a full-width photo gallery feeding the lightbox, followed by a credentials/links footer row. Adam Zimmerman and Eric Ebbole are the two existing artists; new artists should follow this same 3-zone markup structure and add their gallery images to `imgs[]` automatically (the lightbox script wires up any `.artist-gallery img` on the page).
- **Scroll-reveal animations**: elements with `.reveal`, `.reveal-left`, or `.reveal-right` classes (optionally with `.reveal-delay-1..4`) are animated into view via an `IntersectionObserver` in the bottom `<script>` block. Add these classes to new content blocks to keep animation consistent.
- **Lightbox**: all `.artist-gallery img` elements are collected into a single `imgs[]` array for prev/next navigation, independent of which artist's gallery they belong to.
- **External links**: booking goes to Vagaro (`https://www.vagaro.com/redemptiontenfold`), social links go to the studio's Instagram/Facebook, and individual artists link to their own Instagram/portfolio.
- **Responsive breakpoint**: a single `@media (max-width: 960px)` block near the end of the `<style>` section handles mobile layout (hamburger menu, single-column grids, etc.) — keep new layout sections consistent with this breakpoint rather than introducing new ones.

## Commit conventions

Commit messages in this repo are short, imperative, and describe the concrete content/layout change (e.g. "Add real Adam bio, personal IG link, Facebook video embed", "Fix gallery overflow: constrain grid columns so photos stay on screen"). Follow this style: one line, specific about what changed.
