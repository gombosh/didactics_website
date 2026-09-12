# didactics_website

The **didactics.co.il** website for Sivan Gombosh — "להצליח עם חיוך" (remedial teaching,
didactic assessment, and a mentoring program for new remedial‑teaching teachers).

Static HTML/CSS rebuild of the original Wix site
(<https://gombosh3.wixsite.com/didactics>), designed to match its look and content.

## Pages

| File | Page |
|---|---|
| `index.html` | בית (home) |
| `about.html` | אודות |
| `services.html` | שירותים – הוראה מתקנת · אבחון דידקטי · מבדק MOXO |
| `mentoring.html` | תכנית ליווי למורות |
| `workshops.html` | סדנאות |
| `blog.html` | בלוג (static index; posts link to the live Wix blog) |
| `contact.html` | צור קשר |
| `directions.html` | איך מגיעים (Google Maps embed) |

`styles.css` — shared styles for every page.
`assets/` — images and icons exported from the original site.

The home-page intro video is embedded from YouTube: <https://youtu.be/sIdCFNAajic>

## Run locally

```bash
python -m http.server 8000
```

Then open <http://localhost:8000>.

## TODO

- Wire the contact form to a real handler (currently a `mailto:` fallback).
- Migrate full blog‑post bodies if the blog should live here rather than on Wix.
