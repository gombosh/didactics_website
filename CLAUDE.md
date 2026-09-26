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
- **Fonts:** Assistant only, loaded from Google Fonts in each page's `<head>`.
- **Responsive layout:** there is one breakpoint, `@media (max-width:820px)`, at the end of `styles.css`. It collapses the multi-column grids.
- **External embeds:** a YouTube intro video and a Facebook page plugin on the home page, and Google Maps on `directions.html`.
- **Text styling:** most body text is centered (`.center` / `.prose.center`) to match Wix. Key phrases are highlighted with `.red` (a darker red than Wix's, for contrast on the blue/yellow backgrounds) or `.coral`, which work anywhere on the page.
- **Motion** lives in the "Motion" block near the end of `styles.css`. It is all CSS and is off when the visitor's system asks for reduced motion.
  - The menu glides in on a visitor's first page. The inline `<head>` script adds `nav-seen` to `<html>` when the visitor came from another page on the site, which skips the glide.
  - Pages slide between each other using cross-document view transitions.
  - The services page has CSS slideshows (`.slideshow`). Each one repeats its first image as a fourth slide so the loop has no visible jump, and the `slide3` keyframes assume exactly 3 images.
  - The mentoring page plays `assets/mentoring-promo.mp4` on a muted autoplay loop.
- **The blog lives on this site.** `blog.html` is the index; each of the 22 posts migrated from Wix is its own page in `blog/` (English file names, Hebrew content), with images in `assets/blog/`. `blog/archive.html` lists posts by tag, and the sidebar tags link to its anchors (`#tag-<name>`). Post dates are deliberately not shown anywhere (the posts are old); cards and posts show only the reading time. Pages in `blog/` use `../` paths for everything. To add a post: copy an existing `blog/*.html`, then add its card and "recent posts" sidebar entry to `blog.html`, add it to `blog/archive.html`, update the neighbouring posts' newer/older links, and add it to `sitemap.xml`.
- **SEO and AI discoverability:** every page's `<head>` has a canonical URL and Open Graph tags using the clean URLs Cloudflare serves (`https://didactics.co.il/services`, not `services.html`). `index.html` also carries JSON-LD business details (address, phone, email, social links); keep it in sync if those change. `robots.txt`, `sitemap.xml` and `llms.txt` (a plain summary for AI assistants) sit at the root. A new page or post needs its canonical/OG block and a `sitemap.xml` entry.
- **Caching:** `python -m http.server` lets the browser cache pages, so reload with the cache bypassed (hard refresh) after editing.
- **The contact form** posts to Web3Forms (`api.web3forms.com`), which emails sivan@didactics.co.il. The `access_key` in `contact.html` is public by design (it can only send to that inbox). A small inline script submits in the background and shows the result; without JS the form still posts normally.
