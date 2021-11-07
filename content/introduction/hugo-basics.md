---
title: "Hugo Basics"
date: 2021-10-29T15:48:44-04:00
draft: false
lastmodifierdisplayname: DGO
---

Here I'll note some basic hugo commands so I don't have to keep searching the web. The chapter and page commands are from the [Hugo-theme-learn Documentation](https://learn.netlify.app/en/).

Create a new chapter:

```zsh
hugo new --kind chapter <chapter-name>/_index.md
```

This creates a chapter page with the following front matter:

```md
+++
title = "{{ replace .Name "-" " " | title }}"
date = {{ .Date }}
weight = 5
chapter = true
pre = "<b>X. </b>" 
+++

### Chapter X

# Some Chapter title

Lorem Ipsum.
```

Just change the X to the correct chapter number. 

To create a regular page:

```zsh
hugo new <chapter-name>/<page-name>.md
```

This will create a page with the following front matter:

```md
+++
title = "{{ replace .Name "-" " " | title }}"
date =  {{ .Date }}
weight = 5
+++

Lorem Ipsum.
```
