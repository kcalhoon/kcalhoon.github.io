---
title: "How I built this GitHub pages site using Codespace"
date:   2025-04-17 17:30:16 +0000
categories:
  - Projects
tags:
  - website
  - codespaces
  - github
  - jekyll
---

## How I built this site using GitHub Pages and Jekyll

I took inspiration in building GitHub Pages from Simon Willison's [blog post](https://til.simonwillison.net/github-actions/github-pages). Jekyll provides some simple templates. 

However, installing Ruby, Gem, and Jekyll on my local machine (Mac PowerBook Pro M1) did not go well. I almost gave up.

Then I stumbled upon a way to build the site using GitHub codespaces and it worked like a charm. 

I first used the theme Minima but quickly realized I would have to make a lot of modifications to add tags, categories, and a search function. I then found the Minimal-Mistakes theme which has a two column layout and many features built in.

Codespaces is a VS Code interface that is hosted on GitHub. It is a way to code in the cloud. If you are able to set up your local machine, you should be following roughly the same steps. 

### A) To build the site using GitHub Codespaces

1. Set up a GitHub account if you don't have one.

2. Go to the [Quick-Start Guide](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/) for Minimal-Mistakes to gain a basic understanding of how to use the theme. In this page is a link for [Minimal Mistakes remote theme starter](https://github.com/mmistakes/mm-github-pages-starter/generate) 

3. In the Repository name, enter your GitHub username.github.io. For example, mine is kcalhoon.github.io.

4. To start a Codespace, click on the green "Code" button and select "Open with Codespaces". This will open a new browswer link that is a VS Code interface.

5. In the VS Code interface, click on the "Extensions" icon in the left sidebar and search for "Ruby". Install the "Ruby" extension.

6. Install Jekyll and Bundler by running the following commands in the VS Code terminal:

```
bundle config set path 'vendor/bundle'
bundle install
```

7. Install Jekyll by running the following command in the VS Code terminal. This will start the Jekyll server. The --livereload flag will refresh the page when you make changes to the code, add pages, etc. Note changes to _config.yml require you to restart the server using the following command:

```
bundle exec jekyll serve --livereload
```

8. A pop up will give you the option of opening the new site in your browser.

![Popup to open site locally](/assets/images/open_local_jekyll.png)

9. You should see a site like below. You can click around to see the different features and templates. 

![Site rendered locally](assets/images/site_screenshot.png)

10. Go back to the codespace terminal, expand the folders and you can see the files for the pages, posts, images, etc. The various files will provide hints on how to customize the site.

![Folder structure in Codespaces](assets/images/site_folder_structure.png)

11. Use the Minimal Mistakes [site](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/) and the Jekyll [site](https://jekyllrb.com/docs/) to configure and customize the site.

### B) To publish your site externally, do the following:

1. Make sure your GitHub repository is public (or upgrade your subscription). In your GitHub repository, go to Settings > General  > Danger Zone > Change repository visibility (to public)

 

2. Confirm GitHub Pages settings:  
 - Go to your repository on GitHub
 - Navigate to Settings > Pages
 - For "Source", select "GitHub Actions" (not "Deploy from a branch")

3. In the _config.yml file, update the following: 

```
baseurl: "" # as a user site, leave blank
url: "https://yourusername.github.io" # Replace with your GitHub username
```

4. Make sure the Gemfile has the following:

```
gem "jekyll", "~> 3.9.3"
gem "jekyll-paginate", "~> 1.1"
gem "jekyll-sitemap", "~> 1.4"
```
5. Add a .github/workflows/jekyll.yml file (optional but recommended). This enables GitHub Actions to build and deploy your site:

```
name: Build and deploy Jekyll site to GitHub Pages

on:
  push:
    branches: [ main ]

# Sets permissions of the GITHUB_TOKEN
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v4
        with:
          enablement: true  # This will enable GitHub Pages if not already enabled
      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.1'
          bundler-cache: true
      - name: Build with Jekyll
        run: bundle exec jekyll build
        env:
          JEKYLL_ENV: production
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```
6. In your GitHub repository, select Actions. You should see "Update jekyll.yml" with a green checkmark. This means your site is published. In the left sidebar, select Deployments. You should see a green checkmark for the latest deployment and the url for your site.

### C) Troubleshooting

1. In addition to the above sources, I recommend using AI to troubleshoot.

Good luck!