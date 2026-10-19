## Architecture
- Hand-written static HTML site, no framework, no package.json, no build step. Pages: `index`, `about`, `contact`, `gallery`, `inquiry`, `privacy` (`.html` plus a `.md` twin each). Images in `images/`.
- Agent layer: `middleware.js` (Vercel Node middleware) returns the `.md` twin for `Accept: text/markdown` and Markdown 404s; `webmcp.js` registers in-page agent tools; `llms.txt`, `robots.txt`, `sitemap.xml` at root. Keep each `.html` and its `.md` twin in sync by hand.
- `vercel.json` uses clean-URL rewrites (`/about` -> `/about.html`), `Vary: Accept`, and Markdown content-type headers. This repo has no `.vercel` link; the Vercel project is assumed git-connected (unverified).
- No env vars. `.claude/` and `claude-design-skills/` are gitignored.

## Verify
- No install, typecheck, lint or build. Run in order from the repo root:
  1. `node --check middleware.js && node --check webmcp.js` (verified passing).
  2. `python3 -c "import json;json.load(open('vercel.json'))"` (verified passing).
  3. Preview: `python3 -m http.server 8080` -> http://localhost:8080 (static only; rewrites and middleware do not run; use `vercel dev` for those, unverified).
- Then run the teg-site-checker agent on the branch or live URL.
- Live: https://www.tridentphotobooths.ca
