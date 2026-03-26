# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Local Development

**Preferred: Docker**
```bash
docker compose up
```
Serves at http://localhost:4000 with live reload. Uses `_config.yml` + `_config_docker.yml`.

**Alternative: Native Ruby**
```bash
bundle install
bundle exec jekyll serve -l -H localhost
```
Note: `_config.yml` is not hot-reloaded — restart the server after editing it.

## Architecture

This is an [academicpages](https://academicpages.github.io/) Jekyll site deployed to GitHub Pages.

**Content is split into two types:**

1. **Jekyll collections** — Markdown files with YAML front matter, one file per item:
   - `_publications/` — academic papers (front matter: `title`, `venue`, `date`, `paperurl`, `excerpt`, `citation`, `category: [books|manuscripts|conferences]`)
   - `_talks/` — talks/presentations (front matter: `title`, `type`, `venue`, `date`, `location`)
   - `_teaching/` — courses taught
   - `_portfolio/` — project showcases

2. **Static pages** — `_pages/*.md` rendered at fixed URLs (about, CV, publications list, etc.)

**Key config files:**
- `_config.yml` — site identity, author sidebar, navigation-adjacent settings
- `_data/navigation.yml` — top nav bar links (order and visibility)
- `_data/cv.json` — JSON Resume format, powers the `/cv-json/` page

**CV has two versions:**
- `_pages/cv.md` — hand-edited Markdown (rendered at `/cv/`)
- `_data/cv.json` — JSON Resume format (rendered at `/cv-json/`, currently disabled in nav)

## Markdown Generator

`markdown_generator/` contains Python scripts and Jupyter notebooks for bulk-generating collection markdown files from TSV/CSV data:

```bash
# Generate publication markdown files from CSV
python3 markdown_generator/publications.py markdown_generator/publications.csv

# Generate talk markdown files from TSV
python3 markdown_generator/talks.py markdown_generator/talks.tsv
```

## Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/):

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Types:**
- `feat` — new feature
- `fix` — bug fix
- `docs` — documentation changes only
- `style` — formatting, whitespace (no logic change)
- `refactor` — code restructuring (not a fix or feature)
- `chore` — maintenance tasks (deps, config, tooling)
- `ci` — CI/CD changes

**Rules:**
- Use imperative mood: "add X", not "added X"
- Lowercase description after the colon
- Breaking changes: add `!` after type/scope (e.g. `feat!:`) and a `BREAKING CHANGE:` footer
- Keep the subject line under 72 characters

**Examples:**
```
feat: add publications page pagination
fix(cv): correct date format in JSON resume
docs: update local development instructions
chore: upgrade jekyll dependency
```

## CV JSON Sync

`scripts/cv_markdown_to_json.py` syncs `_pages/cv.md` → `_data/cv.json`:

```bash
python3 scripts/cv_markdown_to_json.py -i _pages/cv.md -o _data/cv.json -c _config.yml
```

Or use the shell wrapper:
```bash
bash scripts/update_cv_json.sh
```
