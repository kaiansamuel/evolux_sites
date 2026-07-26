# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is not a software project with a build system — it's a single static
HTML file (`index.html`, 717KB / 2458 lines) that is a browser "Save Page As"
capture of a site called "Vectra Digital Agency", originally built on the
**aura.build** no-code platform. `assets/` (`~250` files) is the raw asset
dump that came with that save — most of it is aura.build editor bagage, not
part of the rendered site (see below).

The active task in this repo, defined in full detail in **`PRD.md`** (in
Portuguese), is to produce a faithful recolored copy of `index.html` for a
company called Evolux: same HTML/CSS/text/animations, but with the
amber/orange/rose accent palette replaced by a green derived from
`icone.jpeg` (`#009B73`). **Read `PRD.md` before doing any work here** — it
is the authoritative spec, including the exact color substitution table,
the list of scripts to strip, and acceptance criteria. Do not duplicate its
content from memory; re-read it, since it may be updated independently of
this file.

There is no package.json, build tool, linter, or test suite in this repo —
work is done by direct HTML/CSS text editing and manual verification in a
browser (e.g. open `index.html` / the output copy directly, or serve the
directory with any static file server such as `python3 -m http.server`).

## Key facts about `index.html`'s structure

- The real page content is plain HTML with **Tailwind CSS already compiled
  and inlined as a static `<style>` block** (captured at save time) — not
  Tailwind classes resolved at runtime, except for one leftover script (see
  below).
- Of the ~260 references to `assets/*` inside `index.html`, only **13** are
  real `src=`/`href=` references used by the rendered page. Everything else
  is referenced only inside a `<script data-offline-runtime="1">` blob (an
  aura.build editor snapshot, ~490KB) that never executes against the live
  page. Use `grep -oE '(src|href)="assets/[^"]*"' index.html | sort -u` to
  re-derive the real list rather than trusting a memorized count.
- Non-rendering scripts present in `index.html` that are editor/tracking
  bagage, not part of the site (see PRD.md §4 for the exact removal list and
  rationale):
  - `<script data-offline-runtime="1">`, `data-offline-resolve="1">`,
    `data-offline-fix="1">`, `data-img-fallback-handler="">` — aura.build
    editor internals.
  - `<script id="aura-supabase-token-firewall">` — Supabase token guard for
    the Vectra account.
  - Google Tag Manager / Google Ads scripts tied to Vectra's tracking IDs.
  - `assets/176e894661aa9cdc_3.4.17` — this is the **Tailwind CDN runtime**
    (`cdn.tailwindcss.com/3.4.17`). Because the real styling is already a
    static compiled `<style>` block, leaving this script active would
    re-run Tailwind's JIT compiler in the browser and silently regenerate
    the *original* amber/orange/rose colors from the class names,
    overwriting any recolor. This makes removing it a functional
    requirement for the recolor task, not just cleanup.
- Interactive/animated behavior that must be preserved byte-for-byte when
  copying (see PRD.md §3 for the full list): 10+ `@keyframes` rules in the
  main `<style>` block (`navDrop`, `logoFade`, `lineReveal`, `ctaRise`,
  `softIn`, `shimmer`, `cms-shimmer`, `scrollMarquee`, `rotateCubeY`, plus
  Tailwind's built-in `pulse`/`ping` utilities), and 7 inline `<script>`
  behaviors: mobile menu toggle (`#mobile-menu-btn`), word-by-word lyric
  reveal (`#lyric-container`), a Three.js WebGL scene (`#webgl-canvas-wow`,
  loaded via ESM from jsdelivr — kept as an external dependency, do not
  vendor it), two typewriter effects (`#typewriter-text-aura-local`,
  `#typewriter-text-aura`), an `IntersectionObserver`-based testimonials
  reveal (`#testimonials` / `#scroll-container`), and a 3D pricing card
  stage (`#pricing .pricing-stage`).

## Working conventions for this repo

- Any recolor/copy work should land in a new sibling directory (e.g.
  `evolux/`), never overwrite `index.html` in place — it's the reference
  original.
- Color substitution is a **global text find/replace** (hex codes and their
  expanded `rgb()`/`rgba()` triplets), not a per-selector CSS rewrite —
  Tailwind's compiled output repeats literal color values in both class
  names (e.g. `text-amber-400`) and arbitrary-value classes (e.g.
  `hover:shadow-[0_0_28px_rgba(251,191,36,0.22)]`), and PRD.md §6 gives the
  exact hex/rgb mapping to preserve alpha/opacity per occurrence.
- Do not insert `logo.jpeg` or `icone.jpeg` as an image anywhere in the
  page — they exist only as the color-extraction source for the palette,
  per PRD.md's explicit scope exclusions (§9).
- Don't rewrite copy/text, restructure sections, or touch layout/breakpoint
  classes — the task is strictly "copy + recolor," not a redesign.
