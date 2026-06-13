# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Build the site
zola build

# Local development server with live reload
zola serve

# Check internal links
zola check
```

Built output goes to `public/` (gitignored).

## Architecture

This is a [Zola](https://www.getzola.org/) static site. Content, templates, and styles are the three moving parts.

### Content → Template mapping

| Content path | Template used |
|---|---|
| `content/_index.md` | `templates/index.html` — homepage, renders album grid |
| `content/albums/_index.md` | `templates/albums.html` — album listing page |
| `content/albums/<name>/index.md` | `templates/album.html` — individual album with photo grid |
| `content/about.md` | `templates/page.html` — generic prose page |

All templates extend `templates/base.html` which provides the nav and footer.

### Adding an album

1. Create `content/albums/<slug>/index.md` with `template = "album.html"` and this front matter shape:

```toml
+++
title = "Album Title"
description = "Short description."
date = 2024-01-01
template = "album.html"

[extra]
cover = "cover.jpg"
photos = [
  { file = "photo1.jpg", caption = "Optional caption" },
  { file = "photo2.jpg", caption = "" },
]
+++
```

2. Place the photos in `static/images/<slug>/` — the slug must match the directory name of the content file.

### Styles

Single file: `sass/main.scss`. Zola compiles it to `public/main.css` automatically. No build step needed beyond `zola build`.

### Deployment

GitHub Actions workflow at `.github/workflows/deploy.yml` triggers on push to `main`, builds with Zola, and deploys via the native GitHub Pages API (`actions/deploy-pages`).

Before first deploy, set `base_url` in `config.toml` to `https://<username>.github.io`, and enable GitHub Pages in the repo settings with source set to **GitHub Actions**.
