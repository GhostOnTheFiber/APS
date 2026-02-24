# Attack Point Security Blog

Source for the [Attack Point Security](https://attackpointsecurity.com) blog, built with [Zola](https://www.getzola.org/) and the [zolanight](https://github.com/mxaddict/zolanight) theme.

## Prerequisites

- [Zola](https://www.getzola.org/documentation/getting-started/installation/) (0.22+)
- Or use the included Nix dev shell: `nix develop`

## Getting Started

Clone the repo with submodules:

```bash
git clone --recurse-submodules https://github.com/your-org/APS-Site.git
cd APS-Site
```

If you already cloned without submodules:

```bash
git submodule update --init --recursive
```

## Usage

```bash
# Start local dev server (http://127.0.0.1:1111)
zola serve --drafts

# Build the site to public/
zola build

# Check for broken links and other issues
zola check
```

Or use the justfile shortcuts:

```bash
just serve   # zola serve --drafts
just build   # zola build
just check   # zola check
```

## Writing a New Post

Create a markdown file in `content/blog/`:

```bash
content/blog/YYYY-MM-DD-post-title.md
```

With frontmatter:

```markdown
+++
title = "Post Title"
date = YYYY-MM-DD
description = "A short summary."
[taxonomies]
tags = ["tag1", "tag2"]
+++

Post content goes here.
```

## Project Structure

```
config.toml          # Zola site configuration
content/             # Markdown content
  _index.md          # Homepage
  blog/              # Blog posts
    _index.md        # Blog section config
static/              # Static assets (copied as-is to output)
templates/           # Template overrides
sass/                # Sass overrides
themes/zolanight/    # Theme (git submodule)
```
