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

## Backlog / next tasks
- P1: Working contact form (saves enquiries to DB) if user wants more than mailto.
- P2: Dedicated project detail pages for the 4 work cases (currently external links).
- P2: Dutch/English language toggle.
- P3: SEO/meta & OG tags for sharing (currently minimal head from export).

## Test credentials
None — public static site, no auth.
