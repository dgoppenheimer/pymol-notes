






Copy `custom-header.html` from `~/Sites/pymol-notes/themes/hugo-theme-learn/layouts/partials` to `~/Sites/pymol-notes/layouts/partials`.

Customize this copy and it will take priority.

```html
<span class="post-date">{{ .Date.Format "Mon, Jan 2, 2006" }}</span>
```

```html
{{ .PublishDate.Format "2 January 2006" }}
```


From [the gohugo discourse site](https://discourse.gohugo.io/t/created-last-updated-date-on-post-academic-theme/20044/3)

```toml
published {{ .Date.Format .Site.Params.dateFormat1 }}
&bull;
updated {{ .Lastmod.Format .Site.Params.dateFormat1 }}
```



In the `config.toml` file, add the following:

```yaml
params:
  dateFormat1: 2006-January-2
```




```toml
frontmatter:
  date: 
    - publishDate
    - :filename
    - date
    - :fileModTime
  publishDate: 
    - publishDate
    - :filename
    - date
    - :fileModTime
  lastmod: 
    - lastmod
    - :fileModTime
    - date
    - publishDate
    ```


In

`_default/simple.html`:

```html
    <span class="post-date">
        {{ with .PublishDate }}
            {{ $.PublishDate.Format $.Site.Params.DateForm }}
        {{ else }}
            {{ $.Date.Format $.Site.Params.DateForm }}
        {{ end }}
    </span>
    ```

From [here](https://discourse.gohugo.io/t/how-to-use-the-publishdate-if-both-publishdate-and-date-are-set-in-frontmatter/5142/2)  
I created this partial called `publishdate-maybe`:

```html
{{ with .PublishDate }}
    {{ if eq ($.PublishDate.Format "2006-01-02") "0001-01-01" }}
        <!-- Print the Date instead of PublishDate if PublishDate is defined but at its initial value of Jan 1, 0001 -->
        {{ $.Date.Format $.Site.Params.DateForm }}
    {{ else }}
        {{ $.PublishDate.Format $.Site.Params.DateForm }}
    {{ end }}
{{ else }}
    {{ $.Date.Format $.Site.Params.DateForm }}
{{ end }}
```



From [here](https://makewithhugo.com/add-a-last-edited-date/)

Add the following to `config.toml`:

```toml
enableGitInfo = true
```

```
[frontmatter]
  lastmod = ["lastmod", ":git", "date", "publishDate"]
```


<div>{{ .Params.date.Format }}</div>

{{ .Date.Format .Site.Params.dateFormat }}

```yaml
frontmatter:
  date: 
    - publishDate
    - :filename
    - date
    - :fileModTime
  publishDate: 
    - publishDate
    - :filename
    - date
    - :fileModTime
```

---

I need to inject the publish date in the body of the content. I can't use the partials that already exist. `custom-header.html` puts its content in the header, and `custom-footer.html` puts its content appropriately at the bottom of the body. 

I'll create a new partial called `publishdate.html` with the content of 

```<div>{{ .Params.date.Format }}</div>
# or
{{ .Date.Format .Site.Params.dateFormat }}
```




---

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
