# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a photo gallery website built with [Hugo](https://gohugo.io/) using the [hugo-theme-gallery](https://github.com/nicokaiser/hugo-theme-gallery) theme (version 4.9.2). The site renders static HTML and is deployed to GitHub Pages.

## Commands

```bash
npm run dev    # Start Hugo dev server with live reload
npm run build   # Build production site (hugo --gc --minify)
```

**Requirements:** Hugo Extended >= 0.123.0 (bundled as `hugo.exe` in the project root on Windows)

## Architecture

### Content Structure (Page Bundles)

Albums are organized as Hugo Page Bundles. Directories with an `index.md` containing images become galleries; parent directories with `_index.md` become album lists.

```
content/
├── _index.md              # Homepage content
├── about.md               # Non-gallery page (no images)
├── animals/
│   ├── _index.md          # Album list page
│   ├── cats/
│   │   ├── index.md       # Gallery page (leaf bundle)
│   │   └── *.jpg
│   └── dogs/
│       ├── index.md
│       └── *.jpg
└── nature/
    ├── index.md           # Gallery with cover image
    └── *.jpg
```

### Key Front Matter Options

- `title` - Album title
- `date` - Album date (used for sorting, newest first)
- `description` - Rendered as markdown on album page
- `weight` - Sort order adjustment
- `params.private: true` - Hidden from album lists and RSS
- `params.featured: true` - Shown on homepage even if private
- `params.theme` - Per-page color theme
- `params.sort_by` / `params.sort_order` - Image sorting (default: by name, asc)
- `resources` with `params.cover: true` - Custom album cover image
- `resources` with `params.hidden: true` - Cover-only images (hidden from gallery)

### Theming

The theme is located in `themes/hugo-theme-gallery-4.9.2/`. To customize:
- Custom CSS: `assets/css/custom.css`
- Custom JS: `assets/js/custom.js`

### Notable Limitations

**Do not use WebP images.** The Go WebP implementation used in Hugo has a bug that produces dull-looking images upon resize.

### Build Output

The `public/` directory contains the generated static site (gitignored, built on each `npm run build`).

### Deployment

The site deploys to `https://zjk159357.github.io/Gallery/` via GitHub Pages. The `.github/workflows/jekyll-gh-pages.yml` handles deployment.
