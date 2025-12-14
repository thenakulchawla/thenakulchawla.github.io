# stdout by Nakul

Personal blog and portfolio website built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

**Live site**: [nakul.net](https://nakul.net/)

## About

This is my personal space for technical writing, focusing on:
- Distributed systems and infrastructure
- Performance engineering
- ML infrastructure and tooling
- Engineering experiments and learnings

## Prerequisites

- [Hugo](https://gohugo.io/installation/) (extended version) - `brew install hugo` on macOS
- Git (for theme submodule management)

## Getting Started

### Clone the repository

```bash
git clone https://github.com/thenakulchawla/thenakulchawla.github.io.git
cd thenakulchawla.github.io

# Initialize theme submodule
git submodule update --init --recursive
```

### Run locally

```bash
# Start development server (includes draft posts)
hugo server -D

# Start server without drafts (production preview)
hugo server
```

The site will be available at `http://localhost:1313/`

The server supports hot reload - changes to content will automatically refresh your browser.

## Project Structure

```
.
├── content/          # Markdown content files
│   └── posts/        # Blog posts
├── static/           # Static assets (images, PDFs, etc.)
│   └── resources/    # Downloadable resources (resume, etc.)
├── themes/           # Hugo themes
│   └── PaperMod/     # PaperMod theme (git submodule)
├── docs/             # Generated site (deployed to GitHub Pages)
├── hugo.toml         # Hugo configuration
├── layouts/          # Custom layout overrides (optional)
├── assets/           # Custom CSS/JS (optional)
└── archetypes/       # Content templates
```

## Common Tasks

### Creating a new post

```bash
hugo new content/posts/my-new-post.md
```

This creates a new markdown file with front matter in `content/posts/`. Edit the file and set `draft: false` when ready to publish.

### Building for production

```bash
hugo
```

This generates the static site in the `docs/` directory with:
- Minified HTML, CSS, and JS
- Fingerprinted assets for cache busting
- Optimized images

### Updating the theme

```bash
git submodule update --remote themes/PaperMod
```

## Deployment

The site is deployed to GitHub Pages from the `docs/` directory on the `main` branch.

**Deployment workflow:**
1. Make changes to content or configuration
2. Build the site: `hugo`
3. Commit both source and generated files: `git add . && git commit -m "Update content"`
4. Push to GitHub: `git push`
5. GitHub Pages automatically deploys from `docs/`

## Configuration

Site configuration is in `hugo.toml`. Key settings:

- **baseURL**: Site URL (https://nakul.net/)
- **publishDir**: Output directory (`docs/` for GitHub Pages)
- **theme**: PaperMod
- **params**: Theme-specific settings (home page content, navigation, etc.)

For theme customization options, see the [PaperMod documentation](https://github.com/adityatelange/hugo-PaperMod/wiki).

## Customization

### Overriding theme templates

Place custom layouts in the `layouts/` directory. Hugo will use these instead of the theme's versions.

### Adding custom styles

Add CSS files to `assets/css/` and they'll be automatically included by PaperMod's asset pipeline.

### Static files

Files in `static/` are copied to the site root. For example, `static/resources/resume.pdf` becomes `https://nakul.net/resources/resume.pdf`.

## License

Content is mine, theme is [licensed by PaperMod](https://github.com/adityatelange/hugo-PaperMod/blob/master/LICENSE).
