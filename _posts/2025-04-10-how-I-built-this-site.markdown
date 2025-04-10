---
layout: post
title:  "How I built this Github pages Jekyll-based site using Codespaces"
date:   2025-04-10 18:02:16 +0000
categories: jekyll site
---

# GitHub Pages Jekyll Setup Guide

This guide documents the successful approach to setting up a GitHub Pages site with Jekyll using GitHub Codespaces, without requiring a local Jekyll installation.

## Prerequisites
- GitHub account
- Repository created for GitHub Pages (can be named `yourusername.github.io` for a user site)
- Basic familiarity with Git and GitHub

## Setup Process

### 1. Open the Repository in GitHub Codespaces
1. Navigate to your GitHub repository in a browser
2. Click the "Code" button
3. Select the "Codespaces" tab
4. Click "Create codespace on main" (or your default branch)

### 2. Ensure You're in the Repository Directory
Verify you're in the correct directory by checking your terminal prompt:
```bash
# Your prompt should look like:
@username ➜ /workspaces/yourusername.github.io (main) $

# If you're in a different directory, navigate to your repository:
cd /workspaces/yourusername.github.io
```

> ⚠️ **IMPORTANT**: Make sure you're in your GitHub repository directory (usually `/workspaces/yourusername.github.io`) and NOT in other directories like `~/jekyll-site`. This ensures your Jekyll site will be properly connected to your GitHub Pages repository.

### 3. Create a Proper Gemfile
Create a Gemfile with the required dependencies:

```bash
cat > Gemfile << EOF
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
gem "webrick"
EOF
```

### 4. Set Up Bundle Configuration
Configure bundle to install dependencies in a vendor directory:

```bash
bundle config set path 'vendor/bundle'
bundle install
```

### 5. Create Basic Jekyll Files
Create a new Jekyll site in the current directory:

```bash
bundle exec jekyll new --force --skip-bundle .
```

### 6. Configure Jekyll for GitHub Pages
Examine the new Gemfile that Jekyll created and ensure:
- The Jekyll gem is commented out (`# gem "jekyll", "~> 4.x"`)
- The GitHub Pages gem is uncommented (`gem "github-pages", group: :jekyll_plugins`)

### 7. Update Bundle and Start Local Server
Update the bundle and start the Jekyll server:

```bash
bundle update
bundle exec jekyll serve --livereload
```

Codespaces will detect the port and offer to open the preview in a browser.

### 8. Verify File Structure
After completing these steps, you should see the following files and directories in your repository:
- `_config.yml` - Site configuration
- `_posts/` - Blog posts directory
- `index.md` or `index.html` - Home page
- `about.md` - About page
- `Gemfile` and `Gemfile.lock` - Ruby dependencies

## Using the Minima Theme (or Other Official GitHub Pages Themes)

GitHub Pages officially supports several themes, including Minima, which is the default. To explicitly set or change themes:

1. Edit your `_config.yml` file:
   ```yaml
   theme: minima
   ```

2. For other officially supported themes, use one of:
   ```yaml
   theme: jekyll-theme-minimal
   theme: jekyll-theme-cayman
   theme: jekyll-theme-slate
   theme: jekyll-theme-hacker
   theme: jekyll-theme-primer
   theme: jekyll-theme-architect
   theme: jekyll-theme-dinky
   theme: jekyll-theme-modernist
   theme: jekyll-theme-leap-day
   theme: jekyll-theme-time-machine
   ```

## Customizing Your Site

### Editing Configuration
Open and edit `_config.yml` to modify:
- Site title and description
- Author information
- Social media links
- Other Jekyll settings

### Creating Blog Posts
Add new posts to the `_posts/` directory with filenames in the format `YYYY-MM-DD-title.md`. For example:
```
---
layout: post
title: "My First Post"
date: 2025-04-10 12:00:00 -0500
categories: jekyll update
---
Your post content here...
```

### Editing Pages
- `index.md` - Edit to customize your home page
- `about.md` - Edit to customize your about page

### Customizing Theme
To customize the default theme:

1. Create a `assets/css/style.scss` file:
   ```scss
   ---
   ---
   
   @import "{{ site.theme }}";
   
   // Add your custom CSS here
   ```

2. Create or modify layouts by adding them to the `_layouts/` directory
3. Add custom includes to the `_includes/` directory

## Workflow for Future Changes

1. Make changes to your files in GitHub Codespaces
2. Preview changes locally with `bundle exec jekyll serve --livereload`
3. Commit and push changes to GitHub:
   ```bash
   git add .
   git commit -m "Update site content"
   git push
   ```
4. GitHub will automatically build and deploy your site

## Troubleshooting

- **Wrong Directory**: If you don't see Jekyll files after setup, make sure you're in your repository directory (`/workspaces/yourusername.github.io`) and not in another directory like `~/jekyll-site`
- **Running Server**: To stop a running Jekyll server, press `Ctrl+C` in the terminal where it's running
- **Permission Issues**: If facing permission issues when installing gems, use `bundle config set path 'vendor/bundle'` to install in a user-writable location
- **Build Failures**: If GitHub Actions fail to build your site, check your `.github/workflows` directory for proper configuration
- **Pages Settings**: Make sure your repository settings are configured for GitHub Pages (Settings > Pages)
