# How to write a new blog post

Everything here can be done in your web browser on github.com. There's nothing
to install.

**Quick version**

1. Open the [`_posts` folder](https://github.com/mattbfrederick/mattbfrederick.github.io/tree/main/_posts)
   on GitHub and click **Add file → Create new file**.
2. Name the file `YEAR-MONTH-DAY-title-of-post.md` using **today's date**, for
   example `2026-11-03-wifi-7-first-impressions.md`.
3. Paste in the [template](#2-fill-in-the-template) below, change the title and
   tags, and write your post underneath.
4. Click **Commit changes…** and then **Commit changes** again.
5. About a minute later it's live at
   `https://finesine.com/2026/11/03/wifi-7-first-impressions/`.

(Until `finesine.com` is pointed at GitHub, the site's address is
`https://mattbfrederick.github.io/` instead.)

The rest of this page explains each step and covers images, formatting, drafts
and fixing problems.

---

## 1. Name the file

The file name decides the post's date and its web address:

```
2026-11-03-wifi-7-first-impressions.md
└─ date ─┘ └──── part of the URL ────┘
```

That file becomes `https://finesine.com/2026/11/03/wifi-7-first-impressions/`.

- Use lowercase letters, numbers and dashes only (no spaces).
- The file must end in `.md` and be inside the `_posts` folder.
- Once a post is published, **don't rename the file**. The address would
  change and old links to it would break.

## 2. Fill in the template

Every post starts with a short settings block between two `---` lines. Copy
this, then replace the example text:

```markdown
---
layout: post
title: "WiFi 7: First Impressions"
tags: [wifi, wireless, wifi 7]
description: "One or two sentences that sum up the post."
---

Your first paragraph goes here.

## A section heading

More text...
```

| Setting | What it does |
|---|---|
| `layout: post` | Always leave this as is. |
| `title:` | The post's title. Keep the quotes, especially if the title has a colon (`:`) in it. |
| `tags:` | Comma-separated, inside `[ ]`. Tags are case-sensitive, so reuse the spelling of existing ones (`wifi`, `aruba`, `ekahau`, `Survey`…). The full list is on the [Tags page](https://mattbfrederick.github.io/tags/). Leave the line out if you don't want tags. |
| `description:` | *Optional.* The summary shown on the home page card and in Google results. Without it, the first paragraph is used. |

## 3. Write the post

Posts are written in **Markdown**, plain text with a few symbols for
formatting:

| To get | Type |
|---|---|
| **Bold** | `**Bold**` |
| *Italic* | `*Italic*` |
| A link | `[Ekahau](https://www.ekahau.com)` |
| A section heading | `## Heading` (use `##` and `###`; a single `#` is reserved for the post title) |
| A bulleted list | Start lines with `- ` |
| A numbered list | Start lines with `1. `, `2. `, `3. ` |
| A quote | Start the line with `> ` |
| `inline code` | Wrap it in backticks: `` `show ap active` `` |
| A horizontal line | `---` on its own line |

Leave a **blank line between paragraphs**. A single line break inside a
paragraph is ignored. To force a line break, end the line with two spaces.

### CLI output and config

Put commands or output between two lines of three backticks. Spacing and
straight quotes are kept exactly as typed, and the block gets the yellow code
tab on the right:

````markdown
```
(Aruba7010) #show ap database
Name         Group     AP Type  IP Address
----         -----     -------  ----------
lab-ap-01    default   535      10.1.1.21
```
````

## 4. Add images

**Upload the image**

1. Rename the image on your computer first, using lowercase letters and dashes
   without spaces (for example `wifi7-heatmap.png`).
2. On GitHub, open the
   [`assets/images` folder](https://github.com/mattbfrederick/mattbfrederick.github.io/tree/main/assets/images),
   click **Add file → Upload files**, drag the image in, and click
   **Commit changes**.

   To keep a year's images together, drag in a whole folder (for example a
   folder named `2026` holding the images) and GitHub keeps the folder.

**Show it in the post**

```markdown
![Heatmap of the second floor](/assets/images/wifi7-heatmap.png)
```

The text in square brackets describes the image for screen readers and search
engines. The part in parentheses is where the image lives in the repository,
starting with a `/`. An image uploaded into an `assets/images/2026` folder,
for example, is `/assets/images/2026/wifi7-heatmap.png`.

An image on a line by itself is centered. On big screens it can be wider than
the text (up to about 1400 pixels), and on phones it shrinks to fit the
screen. Code blocks work the same way: they widen as needed so long CLI output
doesn't have to scroll.

**Optional extras**

```markdown
Show the image smaller (300 pixels wide):
![Heatmap](/assets/images/wifi7-heatmap.png){: width="300"}

Center it:
![Heatmap](/assets/images/wifi7-heatmap.png){: .aligncenter width="300"}

Make it clickable to open the full-size version:
[![Heatmap](/assets/images/wifi7-heatmap.png){: width="300"}](/assets/images/wifi7-heatmap.png)

Add a caption underneath (the caption goes on its own line after a blank line):
![Heatmap](/assets/images/wifi7-heatmap.png)

*Second floor, 5 GHz, -67 dBm coverage*
{: .caption}
```

If you gave the image a `width`, give the caption the same width so it centers
under the image: `{: .caption style="max-width: 300px"}`.

## 5. Publish and check

1. Click **Commit changes…**, add a short message if you like, then click
   **Commit changes** again.
2. GitHub rebuilds the site. Open the
   [Actions tab](https://github.com/mattbfrederick/mattbfrederick.github.io/actions)
   to watch: a yellow dot means it's building, a green check means it's live.
   It usually takes under a minute.
3. Open the home page. The new post is the first card.

If the post doesn't appear, see [Troubleshooting](#troubleshooting).

---

## Editing a post

Open the post's file in `_posts`, click the **pencil icon** (Edit), make your
changes, and commit. The live site updates about a minute later. You can fix
typos in old posts the same way.

## Deleting a post

Open the file, click the **⋯** menu at the top right, and choose
**Delete file**, then commit.

## Saving a draft

Add `published: false` to the settings block:

```markdown
---
layout: post
title: "Not ready yet"
published: false
---
```

The post stays in the repository but isn't shown on the site. Remove that line
(or change it to `true`) when you're ready to publish.

Watch the date in the file name. A post dated in the future is **not**
published. And because GitHub only rebuilds the site when you commit
something, it won't appear by itself when that day arrives. Use today's date
(or an earlier one) when you're ready to publish.

## Writing with a preview

On the repository's main page, press the **`.`** (period) key. That opens a
full editor in your browser (github.dev) with a Markdown preview: right-click
the file and choose **Open Preview**. The preview shows formatting but not the
blog's styling. Commit from the **Source Control** icon on the left.

---

## Troubleshooting

**The Actions tab shows a red ✗.** Click the failed run to see the error. It
almost always comes from the settings block at the top of a post:

- One of the `---` lines is missing.
- The title has a colon but no quotes. Use `title: "Aruba: Something"`.
- A tab character is used for indenting. Use spaces in the settings block.

**The post was committed but doesn't show up.**

- Check that the file is inside `_posts`, not next to it.
- Check that the file name starts with a full date (`2026-11-03-`) and ends in
  `.md`.
- Check that the date isn't in the future. Future-dated posts are skipped
  (see [Saving a draft](#saving-a-draft)).
- Check for `published: false`.
- Refresh with Ctrl+Shift+R (Cmd+Shift+R on a Mac), in case the browser is
  showing an old copy.

**An image shows as a broken icon.** The path doesn't match the uploaded file
exactly. Upper and lower case count. Open the image on GitHub and compare its
path with what's in the post.

**Text with `{{` or `{%` makes the build fail.** The site builder treats
those characters as commands. Wrap that part of the post like this:

```markdown
{% raw %}
text that contains {{ or {%
{% endraw %}
```

---

Other settings live elsewhere. The site title, tagline, header photo and
LinkedIn link are in `_config.yml`. The About page is `about.md`.
