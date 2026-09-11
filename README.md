# KodaiD.github.io

My blog site. Built with [Hugo](https://gohugo.io/). Published at <https://kodaid.github.io>.

## Structure

```
hugo.toml                  Site config (title, affiliation, nav, footer links)
data/publications.yaml     Research output — the single source for the home page and /publications/
content/_index.md          Bio shown on the home page
content/publications.md    /publications/ (contents are generated from the YAML above)
content/blog/*.md          Blog posts
layouts/                   Templates
static/css/style.css       Styles (the only CSS file; no JavaScript anywhere)
.github/workflows/hugo.yml Deployment via GitHub Actions
```

## Setup

```console
$ brew install hugo
```

## Local preview

```console
$ hugo server -D
```

Open <http://localhost:1313>. The `-D` flag includes drafts (`draft: true`).
The browser reloads automatically when a file is saved.

## Adding a publication

Add an entry to `data/publications.yaml`. It appears automatically on both the home
page (3 most recent) and `/publications/` (all entries).

There are four sections:

| Key | Heading |
| --- | --- |
| `journal` | Refereed Journal Papers |
| `conference` | Refereed Conference Papers |
| `talks` | Talks |
| `awards` | Awards |

### Papers (`journal` / `conference`)

```yaml
conference:
  - title: "Paper Title"
    authors: "Kodai Doki, Co Author"   # my own name is bolded automatically
    venue: "Conference name, location"
    year: 2026                          # entries are sorted by this, descending
    pages: "101–106"                    # optional
    links:                              # optional; entries with an empty url are hidden
      - { name: "DOI",    url: "https://doi.org/..." }
      - { name: "PDF",    url: "/pdf/xxx.pdf" }
      - { name: "Slides", url: "https://..." }
```

Renders as:

```
Paper Title
Kodai Doki, Co Author
Conference name, location, 2026, pp. 101–106.
[DOI] [PDF] [Slides]
```

Author-name bolding is driven by `highlightNames` in `hugo.toml`. List every spelling
variant there (e.g. `Kodai Doki` and `K. Doki`).

To host a PDF here, drop it in `static/pdf/` and use `url: "/pdf/xxx.pdf"`.

### Talks and awards (`talks` / `awards`)

```yaml
talks:
  - title: "Name of the talk"
    venue: "Event name"
    year: 2025        # optional; renders as "Event name, 2025"
```

## Writing a post

```console
$ hugo new content blog/my-post.md
```

This creates `content/blog/my-post.md` with `draft: true`. The filename becomes the
URL (`/blog/my-post/`), so use lowercase letters and hyphens.

Front matter:

```yaml
---
title: "Post title"
date: 2026-09-11
draft: true            # remove this line (or set false) to publish
tags: ["databases"]    # optional
description: ""        # optional; used for search results and OGP
---
```

The body is plain Markdown. Code blocks get syntax highlighting.

1. Write with `hugo server -D` running
2. Drop `draft: true` when it is ready
3. Commit and push to `main` — it goes live automatically

## Deployment

Pushing to `main` triggers GitHub Actions (`.github/workflows/hugo.yml`), which builds
the site and publishes it to GitHub Pages. No manual steps.

Pages is already configured to build from GitHub Actions
(**Settings → Pages → Build and deployment → Source**); this only ever needs setting once.

## Changing the look

Layout knobs live at the top of `static/css/style.css` as CSS variables:

| Variable | Purpose |
| --- | --- |
| `--measure` | Max width of the text column (`42rem` narrow / `46rem` default / `52rem` wide) |
| `--base-size` | Body font size — raising it shortens lines without widening the column |
| `--gutter` | Left and right padding |

Colors and fonts are variables in the same `:root` block. Dark mode follows the OS
setting via `prefers-color-scheme`, and its palette is the `:root` block inside the
`@media (prefers-color-scheme: dark)` rule.

Page structure lives in `layouts/`:

- `layouts/index.html` — home page
- `layouts/_default/publications.html` — publications page
- `layouts/_default/list.html` — blog index
- `layouts/_default/single.html` — individual post
