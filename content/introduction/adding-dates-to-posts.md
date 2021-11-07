---
title: "Adding Dates to Posts"
date: 2021-10-22T14:34:02-04:00
draft: true
---

I want to add created date and modified date to each post. My theme does not appear to do this, so I used [this site](https://makewithhugo.com/add-a-last-edited-date/) and the [Hugo documentation](https://gohugo.io/functions/adddate/) as a starting point.

First add the following to the `config.toml`:

```toml
enableGitInfo = true

[frontmatter]
  lastmod = ["lastmod", ":git", "date", "publishDate"]
```

