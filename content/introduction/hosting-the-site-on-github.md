---
title: "Hosting the Site on Github"
date: 2020-09-23T19:04:15-04:00
draft: false
weight: 5
---

I more-or-less followed the instructions provided on the [Hugo website](https://gohugo.io/hosting-and-deployment/hosting-on-github/).

I already have a GitHub account. I'll create a new public repository called `pymol-notes`, which will be the repository for my project. I'll deploy my site from the `gh-pages` branch of my project.

Update the local repository for the site.

```zsh
git add .
git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   .gitignore
	new file:   .gitmodules
	new file:   archetypes/default.md
	new file:   config.toml
	new file:   content/introduction/.DS_Store
	new file:   content/introduction/_index.md
	new file:   content/introduction/building-this-site.md
	new file:   themes/hugo-theme-learn
```

Commit the files.

```zsh
git commit -a -m "first commit of site"
```

Oops. I need to untrack the `.DS_Store` files.

```zsh
git rm --cached content/introduction/.DS_Store
echo ".DS_Store" >> .gitignore
```

## Preparations for _gh-pages_ branch

These steps only need to be done once. Replace `upstream` with the name of your remote; e.g., `origin`:

{{% notice note %}}
Make sure your baseURL key-value in your site's `config.toml` configuration file reflects the full URL of your GitHub pages repository if you’re using the default GitHub Pages URL (e.g., <USERNAME>.github.io/<PROJECT>/) and not a custom domain.
{{% /notice %}}

For this project, in `config.toml`, change `baseURL = "http://example.org/"` to

```toml
baseURL = "https://dgoppenheimer.github.io/pymol-notes/"
```

### Add the _public_ folder to _.gitignore_

Add the `public` folder to the `.gitignore` in your project root.

```zsh
echo "public" >> .gitignore
```

## Initialize your _gh-pages_ branch

Initialize your local `gh-pages` branch as an empty [orphan branch](https://git-scm.com/docs/git-checkout/#git-checkout---orphanltnewbranchgt):

```zsh
git checkout --orphan gh-pages
git reset --hard
git commit --allow-empty -m "Initializing gh-pages branch"
git remote add origin git@github.com:dgoppenheimer/pymol-notes.git
git push origin gh-pages
git checkout master
```

## Build and deploy your project

Checkout the `gh-pages` branch into your `public` directory using the [git worktree feature](https://git-scm.com/docs/git-worktree). This feature allows you to checkout have different branches of the same local repository in different directories.

```zsh
rm -rf public
git worktree add -B gh-pages public origin/gh-pages
```

Regenerate the site using the `hugo` command and commit the generated files on the `gh-pages` branch.

```zsh
hugo
cd public
git add --all
git commit -m "Publishing to gh-pages"
cd ..
```

Or as a script, `commit-gh-pages-files.sh`

```zsh
hugo
cd public && git add --all && git commit -m "Publishing to gh-pages" && cd ..
```

Push the changes to the gh-pages branch to the GitHub repo.

```zsh
git push origin gh-pages
```

### Set _gh-pages_ as your publish branch

To publish the `gh-pages` branch as your site, you need to tweak some setting in GitHub.

- Log into your account using a web browser.
- Go to *Settings* → *GitHub Pages*.
- From *Source*, select *gh-pages branch* and then *Save*.

The site should be visible at [https://dgoppenheimer.github.io/pymol-notes/](https://dgoppenheimer.github.io/pymol-notes/).

Success!

To automate the publishing to `gh-pages` process, create the following script: `publish_to_ghpages.sh`. Put this script in your project directory.

```sh

#!/bin/sh

if [ "`git status -s`" ]
then
    echo "The working directory is dirty. Please commit any pending changes."
    exit 1;
fi

echo "Deleting old publication"
rm -rf public
mkdir public
git worktree prune
rm -rf .git/worktrees/public/

echo "Checking out gh-pages branch into public"
git worktree add -B gh-pages public origin/gh-pages

echo "Removing existing files"
rm -rf public/*

echo "Generating site"
hugo

echo "Updating gh-pages branch"
cd public && git add --all && git commit -m "Publishing to gh-pages (publish.sh)"

#echo "Pushing to github"
#git push --all
```

Make the script executable.

```zsh
chmod +x publish_to_ghpages.sh
```

Run the script with `./publish_to_ghpages.sh`. Either uncomment the `git push` lines, or manually run `git push --all`. Wait a few minutes and the site should be live on GitHub.
