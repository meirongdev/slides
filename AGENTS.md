# AGENTS.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A Hugo static site that renders Markdown into [reveal.js](https://revealjs.com/) slide decks, deployed to GitHub Pages at https://meirongdev.github.io/slides/. There is no application code — content is authored as Markdown with TOML front matter, and the [reveal-hugo](https://github.com/joshed-io/reveal-hugo) theme turns it into presentations.

## Commands

```bash
hugo server          # local dev server with live reload (http://localhost:1313/slides/)
hugo server -D       # include draft pages (front matter draft = true)
hugo --minify        # build the production site into ./public (what CI runs)
hugo mod get -u      # update the reveal-hugo theme module
```

The theme is a **Hugo Module** declared in `go.mod`, not a git submodule — `themes/` is intentionally empty. Hugo fetches it on build, so Go (1.25.1+) and network access are required for a clean checkout. `public/` is the build output; it is git-ignored and should not be hand-edited.

## Deployment

`.github/workflows/deploy.yml` runs on every push to `main`: it builds with `hugo --minify` and publishes `./public` to the `gh-pages` branch. No manual deploy step.

## Content architecture

- **Each top-level directory under `content/` is one slide deck** (a Hugo section). `content/springbootactuator/` is the current example deck; `content/_index.md` is the landing deck.
- **`outputs = ["Reveal"]` in front matter is mandatory** for a page to render as slides. Without it the page falls back to a normal HTML page, not a deck. This output format is defined in `hugo.toml`.
- **`weight` orders slides within a deck.** Files are sorted ascending (the `NN-name.md` numeric prefixes mirror the weights for readability only — `weight` is what actually controls order).
- **Slide separators inside a Markdown file:**
  - `---` (in `hugo.toml` is left as TOML `+++` delimiters; in body) → new **horizontal** slide
  - `--` → new **vertical** slide
  - `{{% section %}} ... {{% /section %}}` → groups the enclosed `---`-separated slides into a vertical stack
- **Images are page-bundle resources**: drop the file next to the Markdown and reference it relatively (`<img src="./repo.png">`), as in `01-enabling.md` / `19-takeaways.md`.
- **Raw HTML in Markdown is enabled** via `markup.goldmark.renderer.unsafe = true` in `hugo.toml` — decks use inline `<div style="...">` for layout.

## Adding a new deck

1. `mkdir content/<topic>` and add `_index.md` with `outputs = ["Reveal"]` (this is the deck's title slide).
2. Add `NN-name.md` files, each with `outputs = ["Reveal"]` and an ascending `weight`.
3. Link it from the relevant list (e.g. `content/home/list.md`) and the `README.md` Slides section.

## Styling & per-deck overrides

- **Site-wide CSS:** `static/css/custom.css` (wired via `custom_css` in `hugo.toml`) loads on every deck.
- **Per-deck CSS / config without touching other decks:** reveal-hugo injects `layouts/partials/<section>/reveal-hugo/head.html` *only* for that section. Drop a `<style>` there to scope CSS to one deck (e.g. `layouts/partials/guardrails/reveal-hugo/head.html` shrinks dense code/table slides). reveal.js options can be overridden per-deck via a `[reveal_hugo]` table in that deck's `_index.md` front matter (e.g. `center = false` to top-align so tall slides aren't clipped).
- **Theme override:** the pinned `reveal-hugo` module references `.Site.Author.name`, removed in current Hugo — `layouts/partials/layout/head.html` is a project-level copy that fixes this (`→ .Site.Params.author`). Without it the build fails on every list page.

## Notes

- `content/_index.md` and `content/home/list.md` both carry the title "Meirong's Presentation"; `home/list.md` is the topic index deck.
