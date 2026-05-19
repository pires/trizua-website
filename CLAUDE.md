# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Jekyll site for the **TriZua** triathlon event in Praia de Mira, Portugal. Hosted on GitHub Pages. All user-facing content is in **European Portuguese (pt-PT)** — preserve the informal, regional tone (e.g. *masso*, *zua*, *fartazana*) when editing copy.

## Build & serve

```bash
bundle install                    # first time / after Gemfile changes
bundle exec jekyll serve          # local dev server at http://localhost:4000 with live reload
bundle exec jekyll build          # one-shot build to ./_site
```

Deployment is automated: pushes to `main` trigger `.github/workflows/jekyll.yml`, which builds with Ruby 3.4.1 and `JEKYLL_ENV=production` and publishes to GitHub Pages. There is no test suite or linter.

## Architecture

- **Theme is `beautiful-jekyll-theme`** pulled in via the `github-pages` gem (see `Gemfile`, `_config.yml`). The theme's layouts/includes live in the gem, **not** in this repo — to customize layout/CSS you'd need to override files locally (none currently overridden). Don't expect to find `_layouts/` or `_includes/` here.
- **Pages are top-level Markdown files** with YAML front matter setting `layout`, `title`, `permalink`, and `cover-img`:
  - `index.md` → `/` (event landing, regulamento, logistics)
  - `standard.md` → `/standard/` (Olympic distance: 1.6k swim / 38k bike / 3×3.6k run)
  - `t100.md` → `/t100/` (T100 distance: 2k swim / 72k bike / 5×3.6k run)
  - `about.md` → `/about/`
  - `404.html` → `/404.html`
- **Navbar links** are declared in `_config.yml` under `navbar-links`. Adding a new page also requires adding it there to appear in navigation.
- **Assets**:
  - `assets/img/` — page covers, course maps, logo. Cover images follow the `praia_mira_panorama*.jpg` naming.
  - `assets/courses/` — Garmin `.fit` files for each distance × discipline. Linked from the discipline sections via `<a href="/assets/courses/...fit">` wrapping the course map image.
- `_posts/` exists with the default Jekyll sample post but is **excluded** from the build via `_config.yml` (this site is not a blog).

## Editing conventions observed in existing pages

- Course descriptions follow a consistent shape: bold distance, road/water conditions, an embedded YouTube `iframe` for the bike, a course map image, and a download link to the matching `.fit` file. Mirror this when adding distances.
- Inline HTML (`<iframe>`, `<img>`, `<a>`) is used freely inside Markdown — kramdown allows it. Image widths are usually set as `width="100%"` or a percentage rather than fixed pixels.
- Cross-page links use absolute paths matching the `permalink` (e.g. `/standard/`, `/about/`), not relative `.md` paths.
