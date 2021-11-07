---
title: "Customizing the Site"
date: 2020-10-09T19:08:25-04:00
draft: false
lastmodifierdisplayname: DGO
---

### Removing the copy-to-clipboard button from inline code

The copy-to-clipboard button is useful on code blocks, but distracting when attached to the inline code. I thought I could remove it using custom `css`, so I created a `static/css` directory in the root of my project and added a `custom.css` file to contain my overrides. To have `hugo` see the custom file, add the following to `config.toml`:

```toml
[params]
    custom_css = ["css/custom.css"]
```

Now I need to figure out what `css` to override.

From searching the web, I found an old issue on the hugo-theme-learn repository, [Configuration of the appearance of the copy-to-clipboard icons](https://github.com/matcornic/hugo-theme-learn/issues/54), where a user suggested using this `css`: 

```css
:not(pre) > code + span.copy-to-clipboard {
    display: none;
}
```

But more importantly, the theme designer, matcornic, updated the theme to provide this function in the theme's configuration file.

Add this to `config.toml` in the `[params]` section:

```toml
[params]
    disableInlineCopyToClipBoard = true 
```

Success!

{{% notice tip %}}
If I would have read the _Configuration_ section of the [Learn Theme for Hugo](https://learn.netlify.app/en/basics/configuration/) documentation, I would have seen the global site parameters where `disableInlineCopyToClipBoard` is mentioned. 
{{% /notice %}}

### Adding author and date to posts

From the [Hugo course by Mike Dane](https://www.mikedane.com/static-site-generators/hugo/archetypes/):

>An archetype is basically a template for default front matter. You can create archetype files and when you use the `hugo new <filename>` command, that default frontmatter will automatically be included.

- Go to the `archetypes` folder at the root of your project folder.
- Open the `default.md` file and add `lastmodifierdisplayname: DGO` to the frontmatter.

Now I'll be added as author on all new documents. At some point I'll add `lastmodifierdisplayname: DGO` to the previous posts.

- Open `config.toml`.
- Add the following to the `[params]` section:

```toml
[params]
    LastModifierDisplayName = true
```

- Copy `_default/single.html` from the theme folder to the project directory to get the following folder structure: `/layouts/_default/single.html`.
- Remove the icons, the `email to:`, and change the date format in `/layouts/_default/single.html`. Change

```hml
{{ partial "header.html" . }}

{{ .Content }}

<footer class="footline">
	{{with .Params.LastModifierDisplayName}}
	    <i class='fas fa-user'></i> <a href="mailto:{{ $.Params.LastModifierEmail }}">{{ . }}</a> {{with $.Date}} <i class='fas fa-calendar'></i> {{ .Format "02/01/2006" }}{{end}}
	    </div>
	{{end}}
</footer>

{{ partial "footer.html" . }}
```

To

```html
{{ partial "header.html" . }}

{{ .Content }}

<footer class="footline">
	{{with .Params.LastModifierDisplayName}}
	    <i class='fas fa-user'></i> {{ . }}, {{with $.Date}}  {{ .Format "02 January 2006" }}{{end}}
	    </div>
	{{end}}
</footer>

{{ partial "footer.html" . }}
```

Success!
