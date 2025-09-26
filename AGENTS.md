# Repository Guidelines

## Project Structure & Module Organization
- `_pages/about.md` holds the homepage content; each new page belongs in `_pages/` with front matter.
- `_layouts/`, `_includes/`, `_sass/`, and `assets/{css,js,fonts}` define the Jekyll theme; prefer reusing includes before writing inline HTML.
- `_config.yml` centralizes global metadata, navigation, and plugin settings; `_data/navigation.yml` controls menu entries.
- `google_scholar_crawler/` contains the automation that syncs Google Scholar stats; its outputs feed the navigation badges under `_data` and the site header.
- `docs/` stores English documentation and screenshots; keep generated assets out of `_site/`, which is build output only.

## Build, Test, and Development Commands
- `bundle install` installs Ruby gems defined in `Gemfile`.
- `bash run_server.sh` (or `bundle exec jekyll serve --livereload`) starts the local dev server at `http://127.0.0.1:4000`.
- `bundle exec jekyll build` produces the static site into `_site/` and is the pre-deploy smoke test.
- `python google_scholar_crawler/main.py` updates citation data when you need to refresh it manually.

## Coding Style & Naming Conventions
- Use 2-space indentation for YAML front matter and config files; mirror existing Liquid tag formatting in `_layouts`.
- Name Markdown files with lowercase hyphenated slugs (e.g., `_pages/research-highlights.md`) and ensure the `permalink` matches the slug.
- Keep SCSS modules under `_sass/` grouped by component; prefix custom classes with `pc-` to avoid theme collisions.
- JavaScript in `assets/js/` should remain modular; wrap new utilities in IIFEs to avoid globals.

## Testing Guidelines
- Run `bundle exec jekyll build` before pushing to catch Liquid or Markdown errors.
- Spot-check critical pages in the browser when the livereload server restarts; confirm scholar metrics render when `_site/assets/` updates.
- For crawler changes, run `python google_scholar_crawler/main.py --help` to validate arguments, then execute against a test scholar ID.

## Commit & Pull Request Guidelines
- Follow the short, imperative commit style already in history (e.g., `add Liu 2022.11`); group related edits together.
- Reference issues in the commit body when applicable and include brief context for data updates.
- Pull requests should summarize user-visible changes, mention affected sections (e.g., `_pages/about.md`), and attach screenshots for layout tweaks.
- Ensure CI/GitHub Pages builds succeed by pushing after a clean `bundle exec jekyll build`; note if the crawler data was refreshed in the PR description.

## Deployment & Automation Notes
- GitHub Pages builds from `main`; avoid committing `_site/`.
- The Scholar crawler workflow writes to `google-scholar-stats`; confirm secrets like `GOOGLE_SCHOLAR_ID` are configured before enabling the action.
- Update favicon assets under `images/` using the generator referenced in `README.md` and keep sizes consistent.
