# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal blog/portfolio website built with Hugo static site generator, using the PaperMod theme. The site is deployed to GitHub Pages via the `docs/` directory.

**Live site**: https://nakul.net/
**Author**: Nakul Chawla - software engineer focused on distributed systems, performance tuning, and ML infrastructure

## Architecture

### Hugo Configuration
- **Config file**: `hugo.toml`
- **Theme**: PaperMod (installed as git submodule in `themes/PaperMod/`)
- **Output directory**: `docs/` (configured via `publishDir = "docs"` in hugo.toml)
- **Base URL**: https://nakul.net/
- **Display mode**: Home-Info mode (configured in `params.homeInfoParams`)

### Directory Structure
- `content/posts/`: Markdown files for blog posts
- `static/`: Static assets (images, PDFs, etc.) - served at site root
- `docs/`: Generated site output (git-tracked for GitHub Pages deployment)
- `themes/PaperMod/`: Theme submodule (read-only, do not modify)
- `layouts/`, `assets/`, `data/`, `i18n/`, `archetypes/`: Hugo override directories (currently minimal/empty)

### Content Organization
- Blog posts are placed in `content/posts/` with front matter:
  ```yaml
  ---
  title: "Post Title"
  date: YYYY-MM-DD
  draft: false
  ---
  ```
- Static resources (resume, images) go in `static/resources/`

## Common Commands

### Development
```bash
# Start local development server with drafts
hugo server -D

# Start server without drafts (production preview)
hugo server

# Server runs at http://localhost:1313/ by default
```

### Building
```bash
# Build site for production
hugo

# Output goes to docs/ directory
# Minified, fingerprinted assets are generated automatically
```

### Content Management
```bash
# Create new blog post
hugo new content/posts/post-name.md

# Creates file with default archetype from archetypes/default.md
```

### Theme Management
```bash
# Update PaperMod theme to latest
git submodule update --remote themes/PaperMod

# Initialize submodules (if cloning fresh)
git submodule update --init --recursive
```

## Deployment Workflow

This site uses GitHub Pages deployment from the `docs/` folder:
1. Make content changes in `content/posts/`
2. Run `hugo` to build static site into `docs/`
3. Commit both content and generated `docs/` files
4. Push to `main` branch
5. GitHub Pages automatically serves from `docs/`

**Important**: Always commit the `docs/` folder after building, as it contains the deployed site.

## Theme Customization

The PaperMod theme is configured via `hugo.toml` parameters:
- `params.homeInfoParams`: Controls home page intro content
- `params.showPostNavLinks`: Enables post navigation
- See [PaperMod wiki](https://github.com/adityatelange/hugo-PaperMod/wiki) for additional configuration options

To override theme templates or assets:
- Place custom layouts in `layouts/`
- Place custom CSS/JS in `assets/`
- Hugo will automatically use local files over theme files

## Content Guidelines

Posts are technical deep-dives focused on:
- Infrastructure and distributed systems
- Performance engineering
- ML infrastructure and tooling
- Engineering experiments and learnings

Writing style: Clear, concise technical writing aimed at clarifying thinking and sharing knowledge.
