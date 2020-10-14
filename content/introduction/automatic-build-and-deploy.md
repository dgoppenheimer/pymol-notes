---
title: "Automatic Build and Deploy using GitHub Actions"
date: 2020-10-09T20:22:44-04:00
draft: false
weight: 30
---

I'm using GitHub Pages to host this site. I started by building the site locally, and pushing the `/public` directory to my `gh-pages` branch on GitHub. (See these instructions on the [Hugo website](https://gohugo.io/hosting-and-deployment/hosting-on-github/).) GitHub provides native support for building [Jekyll](https://jekyllrb.com/) sites, but does not have native support for [Hugo](https://gohugo.io/). Nonetheless, using [GitHub Actions](https://github.com/features/actions), I can build and deploy my Hugo site automatically when I push new content to the `main` branch of my GitHub repository. 

## Getting started

### Rename `master` to `main`

First, rename local branch.

```zsh
cd ~/Sites/pymol-notes
git branch -m master main
git status
On branch main
nothing to commit, working tree clean
```

Next, rename remote branch.

```zsh
git push -u origin main
git push origin --delete master
```

Because I am the solo developer on this project and I have a single local repository, I don't have to do anything else. 

### Automate the build and deploy workflow

First, create a `.github/workflows/` directory in your project root. 

Next, create a `gh-pages.yaml` file in the `.github/workflows/` directory that has the following contents.

```yaml
name: Build and Deploy

on:
  push:
    branches:
      - main

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - name: Checkout main
      uses: actions/checkout@v1
      with:
        submodules: true

    - name: Hugo Deploy GitHub Pages
      uses: benmatselby/hugo-deploy-gh-pages@master
      env:
        HUGO_VERSION: 0.75.0
        TARGET_REPO: dgoppenheimer.github.io/pymol-notes/
        TOKEN: ${{ secrets.EXAMPLE_TOKEN }}
``` 
        
Let's test this.

```
hugo new introduction/test-page.md
```

## Results

No joy. 

Here is a nice tutorial:

[Hugo: Deploy Static Site using GitHub Actions](https://www.google.com/amp/s/ruddra.com/amp/hugo-deploy-static-page-using-github-actions/)

And another set of instructions that uses a similar `.yml` file from `peaceiris`:

[Deploy Hugo project to GitHub Pages with GitHub Actions](https://discourse.gohugo.io/t/deploy-hugo-project-to-github-pages-with-github-actions/20725)

I had success using the `peaceiris` actions

Here is the `gh-pages.yml` file that I used:

```yaml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy

jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.75.1'
          # extended: true

      - name: Build
        run: hugo

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.EXAMPLE_TOKEN }}
          publish_dir: ./public
```

Note: I removed --minify from `run: hugo`, otherwise, my `svg` logo was not visible.


