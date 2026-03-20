# CLAUDE.md

## Project Overview

Personal blog for Attack Point Security (attackpointsecurity.com) built with **Zola** static site generator using the **zolanight** theme (Tokyo Night color palette).

## Key Commands

```bash
just serve   # Dev server with drafts at http://127.0.0.1:1111
just build   # Build static site to public/
just check   # Check for broken links and site issues
```

## Development Setup

- Uses Nix flake + direnv (`direnv allow` or `nix develop`)
- Theme is a git submodule — clone with `--recurse-submodules`

## Project Structure

```
content/           # Markdown content
  _index.md        # Homepage
  about.md         # About page
  blog/            # Blog posts
    _index.md      # Section config (sort by date, paginate by 5)
templates/         # Tera template overrides
static/            # Static assets (includes CNAME)
themes/zolanight/  # Theme submodule
config.toml        # Zola configuration
justfile           # Command shortcuts
```

## Blog Post Conventions

- **Location:** `content/blog/`
- **Naming:** `YYYY-MM-DD-slug-title.md`
- **Frontmatter:** TOML format (`+++` delimiters)

```toml
+++
title = "Post Title"
date = YYYY-MM-DD
description = "Short summary."
[taxonomies]
tags = ["tag1", "tag2"]
+++
```

## Deployment

- GitHub Actions deploys on push to `main` branch
- Uses `shalzz/zola-deploy-action` to build and deploy to GitHub Pages
- Custom domain: attackpointsecurity.com

## Git Workflow

- Working branch: `main`
- PR target branch: `master`
