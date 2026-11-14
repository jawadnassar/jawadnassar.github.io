# Working on jawad.ca

Instructions for anyone (human or LLM) changing this repo. Read this before editing.
If a request conflicts with this file, ask first.

This file is the single source of truth. `CLAUDE.md` imports it and
`.github/copilot-instructions.md` points to it; keep the content here, not there.
For editing post prose, use [`PROOFREADING.md`](PROOFREADING.md) instead of this file.

- **Stack:** Jekyll 4.4.1 static site, GitHub Pages, Monokai dark theme.
- **Build:** `jekyll build` → `_site/` (gitignored). Local preview: `jekyll serve`.
- **Deploy:** push to the Pages branch. `_site/` is never committed.
- **Everything is uncommitted work in progress by default.** Do not `git commit` or `git push` unless explicitly asked.

## Non-negotiables

| Rule | Why |
|---|---|
| **No JavaScript. None.** No inline scripts, no `<script>`, no external JS, no analytics, no cookies. | The site advertises "No JavaScript" on `/about/` and ships `Content-Security-Policy: script-src 'self'`. |
| **Do not edit `css/bsdark.css`.** | It is the vendored Bootstrap dark theme. |
| **All custom CSS lives in `css/style.css`.** | It has `---` frontmatter and a Liquid section at the end. No extra stylesheets, no `extra_css` in frontmatter. |
| **Never hardcode categories, tags, counts, or per-category rules.** | Everything is generated from `site.categories` / `site.tags` at build time. Adding a tag must require zero CSS edits. |
| **Do not touch `404.md`.** | It is intentionally empty by request. |
| **Site name in the nav is plain text**, `<strong>{{ site.name }}</strong>` — never a link. | Explicit preference. |
| **No search box.** | A DuckDuckGo form was added and then removed. Do not re-add it. |
| **The `/posts/` filter bar is not sticky.** | Explicit preference. |
| **Post/page titles stay centered** (`.header`, `h1`, 1em, weight 400). | "Paper article" look. Do not left-align. |
| **Keep the accent pink** `#f92672`. | User picked it over cyan. |
| **Target GitHub Pages (Jekyll 3.10 / Liquid 4.0), not just the local Jekyll 4.4.1.** | The live build uses `github-pages` v232; Jekyll 4-only filters break it. |
| **Minimal diffs.** Don't reformat files, rename unrelated things, or rewrite post content. | Easier review. |

Interactivity is CSS-only: hidden radio inputs + `:has()` selectors. If a feature seems to need JS, it needs a different design.
The only JS allowed anywhere is inside throwaway test harnesses in `_site/` (never in source, never shipped); delete them when done.

## Design language

```
background   #0a0a0a        text        #f8f8f2 (Monokai)
accent       #f92672        muted       #8a8a8a
dividers     #333 (dashed)  borders     #555
code bg      #333           scrollbar   #6a6a6a / #1f1f1f
```

- Body: monospace, 16px, max-width 800px, centered, 1rem padding.
- Accent is a **highlight, not a fill**. Use it for:
  - active nav item (`aria-current="page"`),
  - `::selection`,
  - `:focus-visible` rings,
  - hover borders,
  - small count numbers,
  - `:target` archive sections.
- **Never** paint a large solid pink block. Selected filter chips are a 15% pink tint
  (`rgba(249, 38, 114, 0.15)`) with a pink border and light text.
- Muted `#8a8a8a` for dates, metadata, legends, helper text.
- Labels: `0.8em`, uppercase, `letter-spacing: 0.05em` (e.g. `CATEGORY`, `TAG`).
- Lists: dashed `#333` separators, generous vertical padding, no bullets.
- Dates: `font-variant-numeric: tabular-nums`, right-aligned via
  `grid-template-columns: minmax(0, 1fr) auto`; stack under 560px.
- Nav: lowercase slash style — `/home /posts /tags /categories /about /rss`.
- External links get a `↗` via CSS (`a[target="_blank"]::after`) — don't type it.
- Code blocks: horizontal scroll with a visible thin scrollbar. No edge shadows
  (they read as smudges on the block); the scrollbar is the only hint.
  They scroll on every screen size — never wrap code, it makes it unreadable.

## Semantics and accessibility

- Exactly **one `<h1>` per page**. On posts it is the title in `.header`.
- Heading scale: `h2` 1.35em, `h3` 1.1em, `h4`–`h6` 1em (muted).
- Dates: `<time datetime="{{ post.date | date_to_xmlschema }}">`.
- Active nav link: `aria-current="page"` (conditional on `page.url`).
- Focus rings: `outline: 2px solid var(--accent); outline-offset: 2px`.
- Hidden radio inputs stay keyboard reachable: `position: absolute; width/height: 0; opacity: 0`
  — **not** `display: none`.
- Images: descriptive alt text required, no `alt=""`. Posts reference remote
  `https://jawad.ca/images/...` URLs, but the same files exist locally under `images/`.
- `lang="en-us"`, `data-bs-theme="dark"`, theme color `#0a0a0a`.

## Frontmatter conventions

```yaml
---
layout: post
title: "SQLi UNION attack, retrieving data from other tables"
date: 2023-06-17
series: PortSwigger SQLi        # optional, custom, plain string
categories:
  - CTF
  - PortSwigger
tags:
  - SQLi
---
```

- Permalink: `/:year/:month/:day/:title/` (set globally in `_config.yml`).
- **`series` is not a Jekyll feature.** It is a custom key that groups posts by exact
  string match; the layout sorts the group by date and renders
  `<details>` "Part N of M". Two or more posts are needed for the block to appear.
  A typo in the string silently splits the series.
- A post with no `tags` gets **no related posts**. Related posts are tag-based:
  up to 3, newest first, deduped, current post excluded, **no fallback**.
  Posts in a series often see other parts of the same series as "related".
- New post checklist: copy the frontmatter of a sibling post, change `title`/`date`,
  keep `categories`/`tags`/`series` consistent, add alt text to every image.

## Liquid patterns and pitfalls

These cost real debugging time — reuse them.

```liquid
{% comment %} `empty` is reserved — build an empty array instead {% endcomment %}
{% assign no_items = "" | split: "" %}
{% assign items = no_items %}
{% for post in site.posts %}
  {% assign items = items | concat: post.tags %}
{% endfor %}
{% assign items = items | uniq | sort %}
```

- `empty` is a reserved word in Liquid. `"" | split: ""` gives an empty array.
- `sort` on a Hash returns a single item. Iterate `site.categories` / `site.tags`
  as `[key, posts]` pairs instead.
- Whitespace-only output inside an `unless` block is stripped. Emit `{{ " " }}` when a
  separator space matters.
- `where_exp` works with loop variables; `where: "series", page.series` +
  `sort: "date"` is how series are ordered.
- **`push`, `pop`, `shift`, `unshift` are Jekyll 4-only** and will fail the GitHub Pages
  build. Dedupe with a delimited string plus `concat` instead:

```liquid
{% assign url_key = " " | append: post.url | append: " " %}
{% unless related_urls contains url_key %}
  {% assign related_urls = related_urls | append: url_key %}
  {% assign match = site.posts | where: "url", post.url %}
  {% assign related_posts = related_posts | concat: match %}
{% endunless %}
```

- Guard post-only chrome so pages like `/about/` don't get it:

```liquid
{% if page.collection == 'posts' %} ... {% endif %}
```

- **In `css/style.css`, Liquid output and tag delimiters may only appear in the
  generated-rules section at the end.** The file starts with `---` frontmatter and is
  processed by Liquid.

### Generated filter/count rules

The tail of `css/style.css` loops over `site.categories` / `site.tags` and emits:

- `.post:not([data-categories~="..."]) { display: none }` for each category,
- the same for tags,
- `.filter-empty { display: block }` for impossible category+tag pairs,
- `.result-count-value::after { content: "N posts" }` for every state.

`posts.html` writes `data-categories` / `data-tags` slugs on each `<li>`. Two-ID
`:has()` selectors are used so combined filters override single-dimension rules.
Add a tag to a post and all of this regenerates — do not hand-write rules.

## Build and verification

```bash
jekyll build                 # 4.4.1, user-local, ruby3.2-dev
rm -rf _site .jekyll-cache && jekyll build   # after CSS Liquid or site-data changes
```

Local browser checks:

```bash
python3 -m http.server 4123 --bind 127.0.0.1 --directory _site &
chromium --headless=new --no-sandbox --disable-gpu --dump-dom URL
fuser -k 4123/tcp
```

- Headless Chromium is `/snap/bin/chromium`. **Snap cannot read `/tmp`**, so put test
  harness HTML inside `_site/` and delete it afterwards.
- Harness pattern: an `<iframe>` pointing at the real page, run assertions in
  `iframe.onload` against `contentDocument`, write results into
  `<pre id="test-results">`, read with `--dump-dom --virtual-time-budget=15000`.
- Existing suites (in `/tmp/opencode/`, recreate if missing): posts filters
  (49 checks: every chip, counts, empty states, result text), tags archive,
  categories archive, home desktop+mobile, post page (series/plain/mobile).
- Before reporting done, verify:
  - `jekyll build` clean,
  - filter suite 49/49,
  - exactly one `<h1>` on the page you touched,
  - `git diff --stat -- css/bsdark.css` is empty,
  - CSS braces balanced,
  - no `alt=""` in built posts,
  - no new `<script>`.

### GitHub Pages compatibility

The live site is built by GitHub Pages with `github-pages` v232: **Jekyll 3.10.0**,
Liquid 4.0.4, safe mode, whitelisted plugins only. Local Jekyll 4.4.1 is newer, so a
clean local build does not prove the live build works.

- **No Jekyll 4-only filters** (`push`, `pop`, `shift`, `unshift`). Use `concat` + `where`.
- `jekyll-optional-front-matter` is enabled: any `.md` **without** frontmatter is
  rendered as a page, and Liquid inside it is executed. Keep non-site Markdown out with
  `exclude:` in `_config.yml`, and never leave a bare output/tag delimiter in prose.
- GitHub Pages injects `theme: jekyll-theme-primer`; `_layouts/` overrides it.
- `jekyll-default-layout` / `jekyll-titles-from-headings` may add layouts or titles to
  loose Markdown; explicit `layout:` and `title:` still win.

## Repo map

| Path | Purpose |
|---|---|
| `_layouts/default.html` | Shell: head/CSP, nav, `.header` (h1 + `<time>`), content. |
| `_layouts/post.html` | Post body + footer: categories, tags, series, related, pager, back links. |
| `index.html` | Home: latest 10 posts, tagline, "All N posts →". |
| `posts.html` | `/posts/` list with generated CSS filters and result counts. |
| `tags.html` | `/tags/` archive, `:target` sections, cross-links to categories. |
| `categories.html` | `/categories/` archive, same pattern. |
| `about.md` | About page (uses `layout: post` but is not in the posts collection). |
| `css/style.css` | All custom CSS + generated Liquid rules. |
| `css/bsdark.css` | Vendored Bootstrap dark theme — **read-only**. |
| `_posts/` | Blog posts. |
| `_config.yml` | Site metadata, permalink, three plugins only. |
| `404.md` | Intentionally empty — leave alone. |

## Common tasks

- **Add a post:** new file in `_posts/YYYY-MM-DD-slug.md`; frontmatter as above;
  descriptive alt text on every image.
- **Add a tag:** just add it to posts. Chips, counts, archive section, filter rules,
  and result counts all regenerate. No CSS or config change.
- **Add a nav item:** one `<li>` in `_layouts/default.html` with the same
  `aria-current` conditional and a lowercase slash label.
- **New archive/listing page:** copy `tags.html`, use `layout: default` +
  `permalink`, build the list from `site.*` arrays (never hardcode).
- **Change post footer order:** categories → tags → series → related → pager →
  back links. Keep the `page.collection == 'posts'` guard.

## Rejected on purpose — don't bring these back

- JavaScript of any kind, analytics, cookies, external fonts.
- A separate `css/filter.css`, `extra_css` frontmatter, or any second custom stylesheet.
- A site search form (DuckDuckGo or otherwise).
- Sticky filters on `/posts/`.
- A clickable site name in the nav.
- Solid pink chip fills or any large accent-colored block.
- Content in `404.md`.
- Manual per-category/tag CSS or hardcoded counts.
