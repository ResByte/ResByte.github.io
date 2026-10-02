# resbyte.github.io

Personal blog of Abhinav Dadhich, built with [Hugo](https://gohugo.io) and deployed to GitHub Pages by `.github/workflows/hugo.yml` on every push to `master`.

## Writing a post

Create `content/blog/<yyyy-mm-dd-slug>.md`:

```toml
+++
title = "Post title"
date = "2026-10-02"
description = "One or two sentences — shown under the title and in the post list."
tags = ["ECG", "clinical AI"]
+++
```

- **Math:** `$$ ... $$` on its own lines for display equations, `\( ... \)` inline. Rendered to HTML at build time (KaTeX); no JavaScript.
- **Images:** put them in `static/images/` and reference as `/images/name.png`.
- **Table of contents:** generated from `##`/`###` headings; set `toc = false` to hide it.

## Preview locally

```bash
git submodule update --init   # first time only (theme)
hugo server
```

## Layout

- `assets/css/blog-theme.css` — the Rhythm theme (colours and fonts are the "dials" at the top)
- `assets/css/site.css`, `assets/css/syntax.css` — Hugo-specific additions, code highlighting
- `layouts/` — page templates (override `themes/hugo-bearblog`)
