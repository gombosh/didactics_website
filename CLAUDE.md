# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static HTML/CSS site for **didactics.co.il** (Sivan Gombosh, remedial teaching / didactic assessment / teacher mentoring). It is a hand-built rebuild of the original Wix site (https://gombosh3.wixsite.com/didactics). The pages are meant to match that site's look and content. There is no build step, no package manager, and no tests. The only JavaScript is a one-line script in each page's `<head>` (see Motion below).

## Running locally

```bash
python -m http.server 8000
```

Open http://localhost:8000. `.claude/launch.json` defines this as the `static` preview server.

## Architecture

- **One HTML file per page** at the repo root (`index`, `about`, `services`, `mentoring`, `workshops`, `blog`, `contact`, `directions`). All pages share one stylesheet, `styles.css`. Images live in `assets/`, which were exported from the Wix site.
- **The header, nav, and footer are duplicated in every page** because there are no templates or includes. A change to the nav, footer, phone number, or social links has to be made in all 8 files. Each page marks its own nav link with `class="active"`.
- **The content is Hebrew, right to left.** Every page uses `<html lang="he" dir="rtl">`. Use logical properties such as `padding-inline-start` for anything direction-sensitive, and keep new copy in Hebrew unless asked otherwise.
- **Page backgrounds** are chosen with a class on `<body>`: `bg-yellow` (adds the tiled `doodle-bg.png` pattern), `bg-blue`, or `bg-plain`. The brand colors are CSS variables in `:root` in `styles.css` (`--navy`, `--coral`, `--green`, `--yellow`, `--blue`, `--cream`).
- **Fonts** are Assistant (body text) and Frank Ruhl Libre (serif accents), loaded from Google Fonts in each page's `<head>`.
- **Responsive layout:** there is one breakpoint, `@media (max-width:820px)`, at the end of `styles.css`. It collapses the multi-column grids.
- **External embeds:** a YouTube intro video and a Facebook page plugin on the home page, and Google Maps on `directions.html`.
- **Text styling:** most body text is centered (`.center` / `.prose.center`) to match Wix. Key phrases are highlighted with `.red` (Wix's red) or `.coral`, which work anywhere on the page.
- **Motion** lives in the "Motion" block near the end of `styles.css`. It is all CSS and is off when the visitor's system asks for reduced motion.
  - The menu glides in on a visitor's first page. The inline `<head>` script adds `nav-seen` to `<html>` when the visitor came from another page on the site, which skips the glide.
  - Pages slide between each other using cross-document view transitions.
  - The services page has CSS slideshows (`.slideshow`). Each one repeats its first image as a fourth slide so the loop has no visible jump, and the `slide3` keyframes assume exactly 3 images.
  - The mentoring page plays `assets/mentoring-promo.mp4` on a muted autoplay loop.
- **The blog is a static index.** Each post card, tag and archive entry links to its page on the live Wix blog. The URLs come from the Wix blog feed at `https://gombosh3.wixsite.com/didactics/blog-feed.xml`.
- **Caching:** `python -m http.server` lets the browser cache pages, so reload with the cache bypassed (hard refresh) after editing.
- **The contact form** uses a `mailto:` action. No backend handler exists yet.

## Open TODOs (from README)

- Wire the contact form to a real handler.
- Migrate full blog post bodies if the blog should live here instead of on Wix.
