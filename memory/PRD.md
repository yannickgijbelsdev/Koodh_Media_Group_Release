# PRD — Koodh Media Group website

## Original problem statement
"Hier heb je de html en md file, bouw dit." — User uploaded a fully designed single-file HTML export (643 KB, self-contained: inline CSS/JS, base64 images) plus a markdown content file for Koodh Media Group (audio branding for radio + event photography, based in Budel-Schoot, NL/BE). User choices: exact 1-on-1 rebuild, contact via mailto links only, reuse all embedded images.

## Architecture
- Frontend: React (CRA) dev server serves the original HTML byte-for-byte as `public/index.html`; `src/index.js` is a no-op (no #root, React does not mount).
- Added: `/app/frontend/public/favicon.svg` — k-chevron mark in brand gradient (#B78CFF → #F96F4F) on dark tile, linked from `<head>`.
- Backend: template FastAPI server, untouched (no backend features needed; contact = mailto).
- Database: not used.

## User personas
- Radio station owners/program directors looking for imaging, jingles, sweepers.
- Event/festival organizers looking for photography and drone coverage.

## Core requirements (static)
Sections: hero (animated tunnel + pills), trusted-by strip, selected work (4 cases), services (sweepers & promos, jingles & beds, event photography, drone), about/stats, team (Yannick Gijbels, Chiel van Gansewinkel), FAQ (6 items), contact footer with mailto. Custom animated "koodh media group" SVG logo with REC/timecode motif.

## Implemented
- 2026-10-21: Exact 1-on-1 rebuild served on the Emergent preview; all embedded photos/logos reused; favicon added; verified desktop 1366px, tablet 768px, mobile 375px; backend /api/ healthy.
- 2026-10-24: FAQ section removed on user request (section markup + nav link; unused .faq CSS left in place, harmless).
- 2026-10-24: Trusted-by logo "Radiogroep" replaced with the user-supplied UpLite logo (white background made transparent, embedded as base64); on user request recolored to white to match the other logos.
- 2026-10-24: News integration live — "Selected work" cards fetch from https://clr.koodh.com/api/news/koodh-media-group/homepagina (HTTPS, avoids mixed content; static cards remain as fallback). Article view at /nieuws/<slug> (SPA fallback route in the same index.html) fetches https://clr.koodh.com/api/news/articles/{slug_or_id}, reuses homepage nav/footer/logo, renders title, date, tags, hero image + credit, body HTML incl. audio players. mountTunnel exposed as window.__mountTunnel for the cloned footer (id tunnel3). Duplicate leading image-credit paragraph stripped from body when already shown as figcaption.
- 2026-10-24: Production link bug fixed — root cause: the Clara API does not send Access-Control-Allow-Origin for https://koodhmediagroup.com (only for the preview origin), so on the live domain the news fetch failed and the static fallback cards showed, whose links were absolute https://koodhmediagroup.com/work/<uuid> URLs. Fix: (1) backend same-origin proxy endpoints /api/news/homepagina and /api/news/articles/{id|slug} (60s cache, server-side fetch via requests, no CORS), frontend router fetches proxy-first with direct Clara as fallback; (2) static fallback card links rewritten to relative /nieuws/<slug>; (3) "All work" link now points to #work instead of the external old site. Requires a Redeploy to reach production.
- 2026-10-24: Article meta line (date · category · author) removed on user request; article page now shows title directly above the hero image. Tags row and article:published_time meta remain.
- 2026-10-24: Custom audio player on article pages: native <audio controls> in article bodies replaced by a styled pill player (glass bar, gradient play/pause button, gradient progress fill, mono timecode, click-to-seek, only one plays at a time). Player wiring lives in the article router JS; CSS in the article style block (.aplayer/.aplay/.atrack/.afill/.atime).
- 2026-10-24: Share/SEO setup (farm-seo-setup skill): static head now has title, description, robots meta, theme-color, apple-touch-icon (generated), full OG + Twitter tags (og:image = absolute GRK studio photo from Clara objectstorage, real dims), JSON-LD Organization, noscript fallback, robots.txt + llms.txt in public/. Article router sets per-article document.title, description, og:type=article, og:title/description/image/url, twitter:card=summary_large_image, article:published_time (description = article text with credit/audio stripped, entities decoded). og-image.jpg (1200×630 crop of Genk festival photo) ready in public/ for when a production domain is confirmed. Omitted pending domain: canonical, og:url (static), sitemap.xml. Source assets backed up in /app/scripts/source/.

## Backlog / next tasks
- P1: Working contact form (saves enquiries to DB) if user wants more than mailto.
- P2: Dutch/English language toggle.
- P2: News overview page (grid of all articles) so "All work" stops linking externally.
- P3 (pending production domain): canonical + static og:url + sitemap.xml; swap og:image to /og-image.jpg (1200×630, already in public/).

## Test credentials
None — public static site, no auth.
