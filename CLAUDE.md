# Notes for Claude

FineSine is Matt Frederick's wireless networking blog. It's a Jekyll site built
by GitHub Pages (`github-pages` gem, branch deploy from `main`, repo root) and
served at https://finesine.com. WordPress.com still registers the domain and
hosts its DNS.

## Open item: www.finesine.com certificate (deferred)

`https://www.finesine.com` gives a certificate error
(`NET::ERR_CERT_COMMON_NAME_INVALID`). GitHub's certificates (three
Let's Encrypt certs issued 2026-10-29) cover only `finesine.com`. They were
probably requested while the old `www` record still pointed at WordPress.
`finesine.com` and `http://www.finesine.com` work fine. The owner deferred
this on 2026-10-31 as a rare case, since few people type `www`.

- DNS is correct: `www` CNAME → `mattbfrederick.github.io`, apex A records →
  185.199.108–111.153, and nothing on the domain blocks Let's Encrypt.
- Already tried: removing and re-adding `finesine.com` as the custom domain
  (2026-10-31). No new certificate came within 3 hours.
- Next step, done by the owner in repo Settings → Pages: set the custom domain
  to `www.finesine.com` and save. Wait until **Enforce HTTPS** can be ticked
  (the new cert exists), then set it back to `finesine.com`, save, and tick
  Enforce HTTPS.
- Fallback if switching back loses `www` again: keep `www.finesine.com` as the
  custom domain and change `url:` in `_config.yml` to
  `https://www.finesine.com`.
- To check, search the Certificate Transparency logs with
  `https://api.certspotter.com/v1/issuances?domain=finesine.com&include_subdomains=true&expand=dns_names`.
  crt.sh lags behind. Certs listing dozens of names are old WordPress.com
  certificates and can be ignored.
- Each custom-domain change makes GitHub commit to `CNAME`, so pull before
  pushing.

## Working on the site

- Posting guide for the owner: `HOW-TO-POST.md`. Keep it in step with any
  change that affects how posts are written.
- Design: "Field Notes" (Nov 2026), in `_layouts/`, `_includes/` and
  `assets/main.scss`. Colors, fonts and widths are CSS custom properties at the
  top of `main.scss`. Home page intro, author bio and menu text come from
  `_config.yml`. Post content uses a CSS grid so images, code blocks and
  iframes can be wider than the text. The other two mockups (Spectrum: dark,
  technical; Signal: magazine with photo hero) are in the history of the
  `redesign` branch, commit 5572c9d.
- Old WordPress posts use `{: .caption}`, `{: .aligncenter}` and
  `{: .indent-1}`–`.indent-3`. Keep those classes styled.
- Future-dated posts aren't published.
- `jekyll-optional-front-matter` turns any root `.md` file into a page, so new
  notes files like this one must go in `exclude:` in `_config.yml`.
- Local builds in a cloud session can't reach the GitHub API. Build with an
  extra config that sets `baseurl: ""` and
  `repository: mattbfrederick/mattbfrederick.github.io`, e.g.
  `bundle exec jekyll build --config _config.yml,/tmp/local.yml`.
