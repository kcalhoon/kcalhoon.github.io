# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based blog focused on AI transformation, business strategy, and enterprise AI implementation. The site is hosted on GitHub Pages using the Minimal Mistakes theme and serves as Ken Calhoon's professional blog covering AI industry insights, case studies, and strategic guidance.

## Development Commands

### Local Development
```bash
# Install dependencies
bundle config set path 'vendor/bundle'
bundle install

# Start local development server with live reload
bundle exec jekyll serve --livereload

# Build site for production
bundle exec jekyll build
```

### Deployment
- Automatic deployment to GitHub Pages via GitHub Actions on push to `main` branch
- Manual deployment workflow is defined in `.github/workflows/jekyll.yml`

## Architecture & Structure

### Content Organization
- **Posts**: `_posts/` - Blog posts in markdown format with YAML frontmatter
  - Date-based naming: `YYYY-MM-DD-title.md`
  - Categories: AI-implementation, AI-strategy, newsletter, models, production-ai, etc.
  - Tags for cross-referencing content
- **Pages**: `_pages/` - Static pages (about, archives, etc.)
- **Data**: `_data/navigation.yml` - Site navigation configuration
- **Assets**: `assets/` - Images, PDFs, CSS, and JavaScript files

### Post Structure
All posts follow this frontmatter pattern:
```yaml
---
title: "Post Title"
date: YYYY-MM-DD HH:MM:SS +0000
categories:
  - category-name
tags:
  - tag-name
---
```

### Image and Asset Management
- Images stored in `assets/images/`
- PDFs stored in `assets/pdfs/`
- Use Jekyll's `{% raw %}{% link %}{% endraw %}` syntax for internal asset references
- Custom styling for images with borders and captions available

### Site Configuration
- Primary config in `_config.yml`
- Uses Minimal Mistakes remote theme
- Google Analytics integration (tracking ID: G-G2726SL6HN)
- Pagination set to 5 posts per page
- Author profile and social links configured

## Content Guidelines

### Writing Style
- Focus on enterprise AI transformation and business strategy
- Include data-driven insights and case studies
- Reference authoritative sources (McKinsey, Harvard Business School, Forbes, etc.)
- Use specific metrics and outcomes when available

### Image Usage
```markdown
# Basic image with custom dimensions
![Alt text](/assets/images/filename.png){:height="700px" width="400px"}

# Image with border styling
![Alt text]({% raw %}{% link assets/images/filename.jpg %}{% endraw %}){: style="border: 1px solid #2E8B57;"}

# With caption
![Alt text]({% raw %}{% link assets/images/filename.png %}{% endraw %}){: style="border: 1px solid #2E8B57;"}
<p style="text-align: center; font-size: 0.9em; color: #555;">Caption text</p>
```

### Linking
```markdown
# External links (new tab)
[Link text](https://example.com){:target="_blank" rel="noopener noreferrer"}

# Internal post references (use actual post filename)
[See my post here]({% raw %}{% post_url YYYY-MM-DD-post-title %}{% endraw %})
```

## Theme Configuration

### Minimal Mistakes Theme
- Remote theme: `mmistakes/minimal-mistakes`
- Default skin with custom author profile
- Built-in features: search, categories, tags, pagination
- Archive pages automatically generated
- Social sharing and related posts enabled

### Navigation
- Main navigation defined in `_data/navigation.yml`
- Includes: Posts, Categories, Tags, About
- Footer includes LinkedIn link

## Development Notes

### Dependencies
- Ruby gems managed via Bundler
- Key plugins: jekyll-paginate, jekyll-sitemap, jekyll-gist, jekyll-feed
- GitHub Pages compatible gem set

### Build Process
- Jekyll builds from source to `_site/` directory
- GitHub Actions automatically builds and deploys on push to main
- Local development includes live reload for immediate preview

### Content Management
- Drafts stored in `_drafts/` (not published)
- Archive content in `_archive/`
- Newsletter posts follow consistent naming pattern
- Helper syntax documented in `helpful_syntax.txt`

## File Locations

Key files to understand:
- `_config.yml` - Main site configuration
- `Gemfile` - Ruby dependencies
- `_data/navigation.yml` - Site navigation
- `_posts/` - All blog content
- `assets/images/` - Image assets
- `helpful_syntax.txt` - Markdown and Jekyll syntax reference