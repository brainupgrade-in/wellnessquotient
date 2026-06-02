# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this is

**Wellness Quotient** — a Jekyll blog hosted on GitHub Pages. A luxury
healthcare/wellness "Journal" for a high-net-worth (HNI) readership, modelled on
the positioning of wellnessquotient.community and thewellnessatlas.com.

- **Live:** https://brainupgrade-in.github.io/wellnessquotient/
- **Repo:** `brainupgrade-in/wellnessquotient` · branch `main` (Pages deploys from `main`/root, native Jekyll build)
- **Sitemap (for Google Search Console):** `/sitemap.xml` — auto-generated, never hand-edited
- **Contact CTA everywhere:** Dr. Ujwala — https://t.me/DrUjwala

## Architecture

Jekyll with three Pages-whitelisted plugins: `jekyll-sitemap`, `jekyll-seo-tag`,
`jekyll-feed`. No build step beyond what GitHub Pages runs.

```
_config.yml            Site config + the six/seven pillars + plugins
_layouts/              default · page · post  (post.html = sidebar + main content)
_includes/             head · header · footer · sidebar · post-card  (reusable parts)
_posts/                Blog posts — one Markdown file each
assets/css/style.css   The whole theme (no preprocessor)
assets/js/search.js    Client-side search + pillar filtering over /search.json
assets/images/         favicon.svg + post images
index.html             Landing page
blog.html              Journal index (sidebar + searchable post grid)
about.html             About / Dr. Ujwala
search.json            Generated search index (Jekyll template, sitemap:false)
robots.txt             Points crawlers at the sitemap
```

`sitemap.xml` and `feed.xml` are **generated** — they are not files in the repo.

## Publishing a blog post (the core operation)

**To publish, create ONE Markdown file in `_posts/` — nothing else.** The sitemap,
RSS, search index, homepage "latest", sidebar counts and pillar nav all update
themselves on the next build. **`BLOGGING.md` is the authoritative how-to** (front-
matter template, pillar list, house style, checklist) — read it before adding posts.

- Filename: `_posts/YYYY-MM-DD-slug.md`, date not in the future → URL `/blog/:year/:month/:slug/`
- `categories:` must be exactly ONE pillar: Food · Stress · Exercise · Belongingness · Sleep · Environment · Longevity (defined in `_config.yml` `pillars:`)
- Never hand-edit `sitemap.xml` (it doesn't exist as a file) or the homepage/index to "register" a post.

## Conventions

- **Design tokens** live as CSS custom properties at the top of `style.css`
  (`--forest #16312A`, `--gold #B89B5E`, `--ivory #FAF8F3`; Playfair Display + Inter).
  Reuse them — don't introduce new colours ad hoc.
- **Voice:** considered, literate, evidence-led, unhurried — for affluent readers.
  Editorial guidance, not medical advice (every post carries that framing).
- **Surgical edits:** the templates are deliberately small and shared. Change a
  partial in `_includes/` once rather than duplicating markup.

## Local dev / verification

Toolchain is user-installed (not via the `github-pages` Gemfile, which pins Jekyll 3.x):

```bash
export PATH="$HOME/.local/share/gem/ruby/3.3.0/bin:$PATH"
cd /home/rajesh/wellnessquotient
mv Gemfile Gemfile.hold            # avoid bundler trying to load github-pages
jekyll build --destination /tmp/wq_site
mv Gemfile.hold Gemfile
```

A clean build emits the 3 posts, `sitemap.xml`, `feed.xml`, `search.json` and the
pages with zero warnings. Live URLs can be smoke-tested with `curl -o /dev/null -w "%{http_code}"`.

## Deploy

Push to `main`. GitHub Pages rebuilds in ~1 minute. Pages is already enabled
(Settings → Pages → Deploy from a branch → `main` / root).

### Custom domain (when ready, e.g. wellnessquotient.community)
Set `url:` to the domain and `baseurl: ""` in `_config.yml`, add a `CNAME` file at
root, configure DNS. Sitemap, canonical URLs and feed follow automatically.
