---
title: "Creating a Logo"
date: 2020-09-22T13:50:44-04:00
draft: false
weight: 10
---

I'll create my own logo to replace the beautiful GRAV logo.

Open `PyMOL`.

```py
fetch 3G6M, mol, async=0
extract caf1, /mol//A/CFF`427
h_add
set valence, 0
hide everything, caf1
create molball, caf1
create molstick, caf1
show spheres, molball
set sphere_scale, 0.25
show sticks, molstick
set stick_radius, 0.15
disable mol
color hydrogen, elem H and molball
color wheat, elem C and molball
color salmon, elem O and molball
color lightblue, elem N and molball
color skyblue, molstick
set orthoscopic, off
set field_of_view, 60
set ray_opaque_background, off
viewport 640, 640 
png caffeine-logo.png, width=640, height=640, ray=1
```

I opened this image in Affinity Designer, changed it to black and white, added the text (and rounded the ends of the letters), and saved it as an `.svg` file. I minimized the `.svg` file on the [https://jakearchibald.github.io/svgomg/](https://jakearchibald.github.io/svgomg/) site.

#### Update

The `.svg` file was rather large and didn't look right. I decided to try something different. I used the `.png` file  from PyMOL as a template and created a series of tubes for the bonds and circles for the atoms. I used a 2 pt black stroke and filled the objects with white. It took a while to get the layers in the correct order so the appropriate bonds were on top of the appropriate atoms. I saved the file as a `.svg` file. Without the `.png` embedded as base64, the file was much smaller.

Refer to this post on [Best Practices for Working with SVGs](https://www.bitovi.com/blog/best-practices-for-working-with-svgs) for useful tips.

{{% notice tip %}}
Do not change the `logo.html` file in the `pymol-notes/themes/hugo-theme-learn/layouts/partials/` directory. Instead make a new partial in the `layouts/partials` directory of your local project. This partial will have the priority.
{{% /notice %}}

Create a `partials` directory in the `pymol-notes/layouts/` directory. Copy the `logo.html` file from `pymol-notes/themes/hugo-theme-learn/layouts/partials/` to the `pymol-notes/layouts/partials` directory.

Open the new copy of `logo.html` in your favorite text editor and replace the `svg` code with your new logo.

{{% notice info %}}
The logo size will adapt to different screen sizes automatically.
{{% /notice %}}

Replace the code in between the `<svg>` tags in `logo.html` with the contents of your `.svg` file. Also add the following to the `<svg>` tag:

```html
id="pymol-notes-logo" style="width:100%; height:100%;"
```

To the second and third `<path>` tags, add `fill="#fff"` so that the text is displayed as white.

Save the `logo.html` file, and spin up the site with `hugo server`. Check the site locally at `127.0.0.1:1313/pymol-notes`.

Success!

### Create a favicon

Select the molecule image and paste it into a new Affinity Designer document.

Export it as `favicon.png` at 64 x 52 px (144 dpi resolution).

Place it in the local `static/images/` folder.

Success!