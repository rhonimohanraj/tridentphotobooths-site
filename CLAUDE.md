# tridentphotobooths-site

Trident Photobooths website (photo/video booth rentals, Trident Event Group division). Live: https://www.tridentphotobooths.ca/

## Stack
- Static HTML/CSS/JS, no framework, no package.json. No build step.
- Vercel Edge `middleware.js`: requests with `Accept: text/markdown` get the page's `.md` twin (`/` -> `/index.md`), or a Markdown 404.
- `webmcp.js`: registers WebMCP in-page tools for browser agents (site guide from `llms.txt`, page Markdown, inquiry form). Loaded via `<script src="/webmcp.js" data-brand="Trident Photobooths" data-inquiry="/inquiry.html">`; `index.html` inlines a copy.
- `vercel.json`: clean-URL rewrites (`/about`, `/contact`, `/privacy`, `/gallery`, `/inquiry`), `/:page.html.md` -> `/:page.md`, `Vary: Accept`, Markdown/plain-text content types for `.md` and `llms.txt`.
- Analytics: GA4 inline + Vercel Web Analytics (`/_vercel/insights/script.js`).

## Commands (verified on a1)
```bash
npx serve .                 # local preview with clean URLs (/about works); middleware does not run locally
python3 -m http.server 8000 # alternative; use /about.html etc.
```
No build, lint or test suite exists.

## Verify (run against live URL or a Vercel preview URL)
```bash
python3 ~/TEG-Labs/Automation/seo-audit/seo_audit.py https://www.tridentphotobooths.ca/
python3 ~/TEG-Labs/Automation/design-slop/design_slop.py --path .
python3 ~/TEG-Labs/Automation/copy-check/copy_check.py https://www.tridentphotobooths.ca/
```
Exit codes: 0 clean, 1 warnings, 2 errors. Baseline 2026-10-15: SEO 0 errors / 0 warnings; design-slop flags missing prefers-reduced-motion (fix on branch `a11y/reduced-motion-2026-10-14`, unmerged).

## Layout and conventions
- Every page is a pair: `<page>.html` + `<page>.md` (Markdown twin), linked by `<link rel="alternate" type="text/markdown">` in the HTML head. Pages: index, about, contact, gallery, inquiry, privacy.
- `llms.txt` = agent guide (links the `.md` twins); `sitemap.xml`; `robots.txt`; `images/` = assets.
- Adding a page: create `.html` + `.md` twin, add the alternate link and the `webmcp.js` script tag, add to `sitemap.xml` and `llms.txt`, add a clean-URL rewrite in `vercel.json` if wanted.

## Deploy
- Vercel project `site-tridentphotbooths` (team story93), git-connected.
- Production branch is `danylo/code` (the repo default; there is no `main`). Push to `danylo/code` = prod deploy; other branches get preview URLs.
- Danylo (collaborator) also works on `danylo/code`; branch from it and coordinate.

## Hard rules
- Work on a branch. Never push/merge to `danylo/code` or deploy prod without Rhoni's explicit OK.
- Commit as `Rhoni Mohanraj <rhoni@tridenteventgroup.ca>` (Vercel blocks deploys from other commit authors):
  `git -c user.name="Rhoni Mohanraj" -c user.email=rhoni@tridenteventgroup.ca commit ...`
