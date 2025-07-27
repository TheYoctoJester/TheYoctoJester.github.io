# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based GitHub Pages personal website for Josef Holzmayr ("The Yocto Jester"), a Yocto Project and embedded Linux expert. The site hosts:

- Personal introduction and contact information
- Technical articles about Yocto Project and embedded Linux
- Documentation and resources from livecoding sessions
- Presentation materials and assets

The live site is available at: https://theyoctojester.info

## Development Commands

This is a Jekyll site that uses GitHub Pages' built-in Jekyll theme (`jekyll-theme-minimal`). Since GitHub Pages builds the site automatically, there are no build commands in this repository.

For local development with Jekyll:
```bash
# Install Jekyll and dependencies (if not already installed)
gem install jekyll bundler

# Serve locally (if you have a Gemfile)
bundle exec jekyll serve

# Or serve directly with Jekyll
jekyll serve
```

The site will be available at `http://localhost:4000`.

## Site Structure

- `index.md` - Main landing page with navigation to other sections
- `livecoding.md` - Index of livecoding session resources
- `contact.md` - Contact information and social media links
- `personal.md` - Personal content section
- `articles/` - Technical articles and blog posts
- `livecoding/` - Contains all livecoding session directories
  - `session_*/` - Individual livecoding session documentation with companion resources
- `presentations/` - Presentation materials and associated assets
- `images/` - Static image assets
- `context/` - Contains documents, sources, and examples relevant for operations on page content (read these for context when needed, but don't memorize)
- `_config.yml` - Jekyll configuration with theme and site metadata

## Content Organization

### Livecoding Sessions
Each session is organized in its own directory under `livecoding/` (`livecoding/session_*`) containing:
- `main.md` - Session notes, links to recordings, and external resources
- Additional assets as needed

### Articles
Technical articles are stored in the `articles/` directory as individual Markdown files.

### Presentations
Presentation materials are organized by event/date under `presentations/` with associated asset directories.

## Key Files

- `_config.yml` - Jekyll configuration, site title, description, and theme settings
- `CNAME` - Custom domain configuration for GitHub Pages
- `.gitignore` - Excludes Jekyll build artifacts and system files

## Content Guidelines

When editing content:
- Maintain the existing informal, personal tone
- Use relative links for internal navigation
- Keep technical content focused on Yocto Project, OpenEmbedded, and embedded Linux
- Include YouTube links for livecoding session recordings where available
- Use consistent Markdown formatting for session documentation