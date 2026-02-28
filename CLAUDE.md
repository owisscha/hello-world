# CLAUDE.md

This file provides guidance for AI assistants working in this repository.

## Repository Overview

**hello-world** is a personal GitHub Pages site belonging to Onno Wisscher (`owisscha`). It serves as a homepage and introduction to their Deep Learning projects, and was created as a first exercise in using Git.

The site is rendered via **GitHub Pages** using the **Jekyll** static site generator with the `jekyll-theme-minimal` theme.

## Repository Structure

```
hello-world/
├── CLAUDE.md        # This file — AI assistant guidance
├── README.md        # Repository description shown on GitHub
├── _config.yml      # Jekyll site configuration
└── index.md         # GitHub Pages homepage content (takes precedence over README)
```

### File Roles

| File | Purpose |
|---|---|
| `README.md` | Displayed on the GitHub repository landing page. Contains a brief intro and motivation. |
| `index.md` | The GitHub Pages homepage. GitHub Pages renders this as the site's front page (`/`). |
| `_config.yml` | Jekyll configuration: sets the theme, site title, and description. |

## Key Conventions

- **Markdown only** — all content files use `.md` (Markdown). No HTML, JavaScript, or CSS files are present; styling is handled entirely by the chosen Jekyll theme.
- **Jekyll front matter** — if adding pages or posts, use YAML front matter (between `---` delimiters) at the top of `.md` files to set layout, title, or other metadata.
- **Theme** — the site uses `jekyll-theme-minimal`. Avoid overriding theme styles unless absolutely necessary; prefer Jekyll's built-in configuration options.
- **No build system** — there is no `Gemfile`, `package.json`, or local build tooling. GitHub Pages handles building automatically on push to `master`.

## Development Workflow

### Branches

| Branch | Purpose |
|---|---|
| `master` | Production branch — GitHub Pages serves content from here |
| `claude/*` | Feature/task branches used by AI assistants |

### Making Changes

1. Work on a `claude/<description>` branch (never commit directly to `master`).
2. Edit `.md` files or `_config.yml` as needed.
3. Commit with a clear, descriptive message.
4. Push the branch and open a pull request targeting `master`.

### Previewing Locally (optional)

Because the site uses GitHub Pages with a supported theme, you can preview it locally with Jekyll:

```bash
gem install bundler jekyll
jekyll serve
# Visit http://localhost:4000
```

No `Gemfile` is checked in, so this is optional and not part of the standard workflow.

## GitHub Pages Configuration

- **Source branch:** `master`
- **Theme:** `jekyll-theme-minimal` (set in `_config.yml`)
- **Site title:** "Welcome to Onno's homepage!"
- **Site description:** "overview of my Deep Learning projects"

The live site URL follows the standard GitHub Pages pattern:
`https://owisscha.github.io/hello-world/`

## Content Guidelines

- Keep content concise and focused on Deep Learning projects.
- `index.md` is the primary content page visible to site visitors.
- `README.md` is for GitHub visitors (repository context), not site visitors.
- When adding new project pages, create new `.md` files and link to them from `index.md`.
