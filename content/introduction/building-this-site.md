---
title: "Building this site"
date: 2020-09-22T13:50:44-04:00
draft: false
weight: 1
---

I already have [Hugo](https://gohugo.io/) installed. I installed it using [MacPorts](https://www.macports.org/).

### Create a project

```zsh
cd ~/Sites
hugo new site pymol-notes
```

### Install the theme

I want to use the [Learn theme](https://themes.gohugo.io/theme/hugo-theme-learn/en/) for Hugo. It is a theme designed for documentation. It looks nice and is functional.

The theme's repository is: [https://github.com/matcornic/hugo-theme-learn.git](https://github.com/matcornic/hugo-theme-learn.git).

```zsh
cd pymol-notes
git init
git submodule add https://github.com/matcornic/hugo-theme-learn.git themes/hugo-theme-learn
echo 'theme = "hugo-theme-learn"' >> config.toml
```

### Configure the theme

Open `config.toml` and add the following:

```zsh
# Change the default theme to be use when building the site with Hugo
theme = "hugo-theme-learn"

# Change the title
title = "PyMOL Notes"

# For search functionality
[outputs]
home = [ "HTML", "RSS", "JSON"]
```

### Create a chapter page

```zsh
hugo new --kind chapter introduction/_index.md
```

### Create some content

```zsh
hugo new introduction/building-this-site.md
```

### Spin up the site

```zsh
hugo server
```

{{% notice note %}}
When you run `hugo server`, Hugo will build the site in and serve the pages from memory.
{{% /notice %}}

Go to `http://localhost:1313`.

Success!

{{% notice note %}}
If you want the site to be built in, and served from, the `public` directory, then run `hugo server --renderToDisk`. If you just want to build the site in the `public` directory, then run `hugo`.
{{% /notice %}}

### Create a homepage

```zsh
hugo new _index.md
```

Add some content to the homepage, and you're good to go!

### Add another chapter page

```zsh
# make sure you are in the pymol-notes directory
cd ~/Sites/pymol-notes
hugo new --kind chapter GROMACS-simulations/_index.md
```

Open the new `_index.md` file and change the front matter and add some text. 


Save the files, and track them with `git`.

```zsh
git status
git add .
git commit -m "added a new chapter on gromacs"
# deploy the new pages
git push origin/main
```