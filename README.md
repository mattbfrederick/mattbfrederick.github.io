# FineSine

The [finesine.com](https://finesine.com) blog, moved from WordPress.com to a
[Jekyll](https://jekyllrb.com) site that GitHub Pages builds and hosts for free.
The design, "Field Notes" (2026), is editorial and quiet: a warm off-white
page, Newsreader serif for reading, Inter for menus and labels, and one
burnt-orange accent. The home page lists every post by year. The design lives in
`_layouts/`, `_includes/` and `assets/main.scss`. Its colors, fonts and widths
are the settings at the top of `assets/main.scss`.

## Publish it on GitHub Pages

1. Create a new **public** repository named **`mattbfrederick.github.io`**.
   That name makes GitHub serve the site at the root of
   `https://mattbfrederick.github.io/`, so you can check everything before
   moving the domain. Any other name works too, but then images and links
   inside posts only load once `finesine.com` points at GitHub.
2. Push this folder to it:

   ```bash
   git remote add origin https://github.com/mattbfrederick/mattbfrederick.github.io.git
   git push -u origin main
   ```

3. In the repository, go to **Settings → Pages**. Under **Build and deployment**,
   choose **Deploy from a branch**, branch **`main`**, folder **`/ (root)`**, and save.
   The first build takes a minute or two (watch the **Actions** tab).
4. Open `https://mattbfrederick.github.io/` and look around.

## Move finesine.com to GitHub

Do these in order. GitHub asks you to add the domain on its side *before*
changing DNS, so nobody else can claim it in between.

1. *(Recommended)* Verify the domain: your GitHub profile **Settings → Pages →
   Add a domain**, then add the TXT record it gives you at WordPress.com (step 3).
2. Repository **Settings → Pages → Custom domain**: enter `finesine.com` and save.
   GitHub commits a `CNAME` file to the repo for you.
3. At WordPress.com, open **my.wordpress.com/domains → finesine.com → DNS records**:
   - Delete the existing `A`/`AAAA` records for the root domain (`@`) and any
     record for `www`, since they point at WordPress. Leave email (`MX`) records alone.
   - Add four `A` records for `@`:
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - Optional IPv6, four `AAAA` records for `@`:
     `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`
   - Add a `CNAME` record: `www` → `mattbfrederick.github.io`

   If WordPress.com won't let you change the root records while the domain is
   still attached to the blog, see their *Edit DNS records* help page or ask
   their support.
4. Wait for DNS to update (usually within an hour, sometimes up to a day or two).
   When **Settings → Pages** shows the domain check passing, tick **Enforce HTTPS**.
5. Once `https://finesine.com` is serving the new site, cancel the WordPress.com
   *plan* but keep the *domain*. Before you cancel, download a backup from
   WordPress.com (**Tools → Export**, both content and media library).

## Write a new post

See **[HOW-TO-POST.md](HOW-TO-POST.md)** for step-by-step directions. In short,
add a file to `_posts/` named `YEAR-MONTH-DAY-title-with-dashes.md`, starting
with:

```markdown
---
layout: post
title: "My new post"
tags: [aruba, wifi]
---
```

Write the post below that in Markdown and commit it. The live site updates
about a minute later.

To edit the About page, change `about.md`. These are in `_config.yml`:

- `intro_title` and `intro_text`: the big headline and sentence at the top of the
  home page
- `author_bio`: the "about the author" note at the end of every post
- `navigation`: the menu at the top of every page
- `description`: the one-line summary search engines and feed readers show
- `title` and `linkedin_url`

## Preview on your own computer (optional)

With [Ruby](https://www.ruby-lang.org) installed, run this after adding the
GitHub remote above:

```bash
bundle install
bundle exec jekyll serve
```

Then open <http://localhost:4000>.

## What changed in the move

- **URLs are the same** for every post (`/2016/01/14/post-name/`) and `/about/`.
  Old tag pages (`/tag/wifi/`), `/page/2/` and `/category/uncategorized/`
  redirect to their new equivalents. The Blogroll page was removed, and
  `/blogroll/` now redirects to the home page.
- **RSS feed** is now `/feed.xml`. The old `/feed/` address redirects in a
  browser, but feed readers may need the new URL.
- **Comments.** The 11 existing comments are kept, read-only, under their posts
  (in `_data/comments/`). One spam comment ("best vpn for canada") was left out.
  New comments are off. [giscus](https://giscus.app) can add them back using
  GitHub Discussions.
- **CLI output** in the Aruba posts is in code blocks now, with the straight quotes
  and dashes that WordPress had turned into curly quotes and em dashes, so
  copy-paste works again.
- **Images** live in `assets/images/YEAR/MONTH/`. Posts that showed a
  WordPress thumbnail use a resized copy (`-600w` etc.) that links to the
  full-size original.
- **Design.** The site first moved over with a copy of the old WordPress
  theme (Dyad). It was redesigned in November 2026. The Tags page is now called
  Topics but keeps its `/tags/` address.
- **WordPress followers** (7) don't carry over. You can export the list from
  WordPress.com if you want to let them know about the move.
