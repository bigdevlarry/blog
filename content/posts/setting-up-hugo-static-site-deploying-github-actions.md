---
title: Setting Up a Hugo Static Site and Deploying with GitHub Actions
date: 2024-10-13
description: "Hugo is a static site generator known for its speed and flexibility, making it a great choice for building fast, modern websites."
image: images/hugo-gh-action/gh-logo.png
imageAltAttribute: Github Actions for Hugo
tags:
   - hugo  
   - gitHub actions
---


## Why Hugo?

Using Hugo was a personal choice because I wanted something blazing fast, with easy setup and no dependencies. It will serve static sites, and the markdown support makes it perfect for a straightforward blogging experience.

<a href="https://gohugo.io/">Hugo</a> is a static site generator known for its speed and flexibility, making it an excellent choice for creating fast, modern websites.

In this guide, you’ll learn the following 

- How to use Homebrew to install Hugo
- Installing a Hugo theme of your choice
- Hosting and Deployment with Github Actions


## How to use Homebrew to install Hugo
**Homebrew** is a free, open-source package manager that allows users to install, update, and manage software on macOS and Linux. 
You can install it by following this  <a href="https://brew.sh/" target="_blank"> link.</a>

With Homebrew installed, proceed to install Hugo on your system using the command line or terminal. Executing the below command fetches the latest version of Hugo from the official repositories and sets it up for use.

```
brew install hugo
```

Next, navigate to your preferred work directory and create a new Hugo site in a directory called `myblog`. This will be an executable project to get you up and running.

``` 
hugo new site myblog
```

Navigate to the Hugo site using the command. You have to be within this directory to enable you to execute all Hugo commands.

```
cd myblog
```

Start a local development server. You can preview your site in a web browser at `http://localhost:1313`. Hugo also ships with hot reload, ensuring that the page automatically refreshes when you make local changes to your content.

```
hugo server
```


## Installing a Hugo theme of your choice
Hugo has a nice open-source collection of themes available at <a href="https://themes.gohugo.io/themes">themes.gohugo.io/themes</a>. For most themes, there’s always a clear setup instruction, while others might leave you wondering what to do next.

I’ll install this <a href="https://themes.gohugo.io/themes/hugo-winston-theme/">Hugo Winston theme</a>. I also learnt there are two ways to install a theme, either as a Hugo module or Git submodule, so I’ll be doing the latter. 
But to do that, we need to do the following:

### Create a GitHub repository
You'll need to create a git repository for the project by following this URL <a href="https://github.com/new"> https://github.com/new </a> <img src="/blog/images/hugo-gh-action/gh-repo.png" alt="Image of GitHub New Repository"/>
### Setup GitHub pages and deployment source
Set up GitHub pages, which we will use to host the static sites by going to this URL:
```
https://github.com/<your_github_username>/<repository_name>/settings/pages
```
In the **Build and Deployment** section of the GitHub pages, select GitHub Actions from the source dropdown <img src="/blog/images/hugo-gh-action/gh-pages.png" alt="Image of GitHub pages"/>

### Clone the theme (hugo-winston-theme)
Click the **Download** button from the <a href="https://themes.gohugo.io/themes/hugo-winston-theme/"> Hugo Winston theme</a>, which will redirect you to the project's GitHub repository.
Fork the repository, and you should have a copy of it in your repository <img src="/blog/images/hugo-gh-action/gh-fork.png" alt="Image of GitHub fork repository"/>

### Create the submodule locally
Next, add your forked version of the theme as a new submodule using the command below. This step integrates the theme as an external repository into your current Git repository. After executing the command, you'll find the theme files located in the themes/hugo-winston-themes directory.
```
git submodule add <your-forked-theme-repo-url> themes/hugo-winston-themes
```
Replace <your-forked-theme-repo-url> with the URL of your forked repository. For example, if your GitHub username is **myusername** and the theme is named **hugo-winston-themes**, the URL would look like:
```
git submodule add https://github.com/myusername/hugo-winston-themes themes/hugo-winston-themes
```

Commit the changes to your Hugo site's repository
```
git add .
git commit -m "Updated to use the forked theme"
git push origin main
```
## Hosting and Deployment with GitHub Actions
Hugo provides a comprehensive guide for <a href="https://gohugo.io/hosting-and-deployment/hosting-on-github/">hosting and deployment</a>, which you can refer to for more details. Alternatively, you can follow along here as we create a `.github/workflows/hugo.yaml` file in the repository to set up automated deployment.

**hugo.yaml**
```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.134.2
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

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
Commit your latest changes to the local repository with a relevant commit message, such as "Add Hugo workflow file", and push them to GitHub. Then, navigate to your project's GitHub repository, click the **Actions** tab, and you’ll see the deployment process in progress.
<img src="/blog/images/hugo-gh-action/gh-actions.png" alt="Image of GitHub action deployment"/>

You’ll find a link to your live site in the deploy step above. Moving forward, every time you push changes from your local repository, GitHub will automatically rebuild and deploy your updated site.

### Conclusion 
In this article, you have learned about Hugo, relevant commands, its collections of themes, and how to deploy to GitHub Pages.
You can find the open-source code for this blog at https://github.com/bigdevlarry/blog. 

If you found this helpful, feel free to star the repository ⭐!

Keep learning, and Happy Coding!