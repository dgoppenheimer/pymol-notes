---
title: "Customizing the Site"
date: 2020-10-09T19:08:25-04:00
draft: false
---

### Removing the copy-to-clipboard button from inline code

The copy-to-clipboard button is useful on code blocks, but distracting when attached to the inline code. I thought I could remove it using custom `css`, so I created a `static/css` directory in the root of my project and added a `custom.css` file to contain my overrides. To have `hugo` see the custom file, add the following to `config.toml`:

```toml
[params]
    custom_css = ["css/custom.css"]
```

Now I need to figure out how what `css` to override. 

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