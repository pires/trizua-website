# TriZua

Jekyll site for the **TriZua** triathlon in Praia de Mira, Portugal — Edição UM, 31 May 2026.

Published at <https://trizua.com> via GitHub Pages from the `main` branch. All user-facing content is in pt-PT (pre-1990 orthography); developer-facing docs (this file, `CLAUDE.md`) are in English.

## Local development

Two options: use Podman via the Taskfile (no Ruby toolchain needed) or install Ruby/Bundler natively.

### Podman + Task (recommended)

Prerequisites:

- [Podman](https://podman.io) — on macOS, after installing, start the VM once: `podman machine init && podman machine start`.
- [Task](https://taskfile.dev) — `brew install go-task` on macOS.

Commands:

```bash
task                  # list available targets
task install          # install gems into ./vendor/bundle (runs automatically as a dep)
task serve            # dev server with live reload at http://localhost:4000
task build            # one-shot production build to ./_site
task update           # refresh Gemfile.lock (commit the result)
task shell            # open a bash shell inside the container for debugging
task clean            # remove _site and Jekyll caches
task clean-all        # also remove vendor/ (forces a full reinstall next time)
```

The first `task serve` pulls the `ruby:3.4.1` image and installs the gems — expect a few minutes. Subsequent runs are near-instant because gems are cached in `./vendor/bundle` (gitignored).

### Native Ruby

```bash
bundle install
bundle exec jekyll serve
bundle exec jekyll build
```

## Maintenance

- **Add/remove gems**: edit `Gemfile`, run `task install` (or `bundle install`). Commit both `Gemfile` and `Gemfile.lock`.
- **Update gems**: `task update` (or `bundle update`). Verify the site locally before committing the new `Gemfile.lock`.
- **Change the Ruby version**: update `IMAGE` in `Taskfile.yml` **and** the matching version in `.github/workflows/jekyll.yml` so local and CI stay aligned.
- **Add a new page**: create the `.md` at the repo root with front matter (`layout`, `title`, `permalink`, `cover-img`) and add an entry under `navbar-links` in `_config.yml` if it should appear in navigation. See `CLAUDE.md` for editing conventions.
- **Deploy**: automatic — pushing to `main` triggers `.github/workflows/jekyll.yml`, which publishes to GitHub Pages.

## Layout

Quick summary — see `CLAUDE.md` for architecture details and editing conventions.

- `index.md`, `about.md`, `404.html` — top-level pages.
- `_config.yml` — Jekyll and `beautiful-jekyll-theme` configuration.
- `assets/img/` — cover images, course maps, logo.
- `assets/courses/` — Garmin `.fit` files for each distance × discipline.
- `Taskfile.yml` — Podman-based targets described above.
- `.github/workflows/jekyll.yml` — GitHub Pages build and deploy.
