# Deploy check: Terafab Command Center (GitHub Pages)

Source: `final/terafab-command-center.html` (r9, Dikastes PASS). Copied to `deploy/index.html`. Content, copy, design and behaviour are unchanged. Date: 2026-11-20.

## Changes made (deployment and compatibility only)
1. `<head>`: added `<meta name="description">`, `<meta name="robots" content="noindex, nofollow">` and an inline SVG data-URI favicon (stops the `/favicon.ico` 404).
2. No-WebGL fallback: added `!R` guards to `camSet()`, `paintWafer()` and the `setScene()` transition. Before this, browsers without WebGL showed the "WebGL unavailable" note but then threw 8 `TypeError`s when you used Watch or Wafer. With WebGL available, nothing changes.

## Results

| # | Check | Result | Notes |
|---|---|---|---|
| 1 | Self-contained | PASS | No `file://`, `/Users/…`, `app/`, `qa/` or `vendor/` references. The `http(s)://` strings are: `http://www.w3.org/2000/svg` and `http://www.w3.org/1999/xhtml` (XML namespace identifiers, never fetched), and `https://discourse.threejs.org/t/updates-to-lighting-in-three-js-r155/53733` ×2 (text inside a three.js warning message, never fetched). The only `href=` is the inline favicon. There are no `src=` attributes; the remaining `src=`/`href=` matches are inside the inlined three.js code. 0 network requests besides the page itself. |
| 2 | Relative/inline assets, font fallback | PASS | All CSS, JS (three.js) and SVG are inline. Font stacks: `"SF Pro Text","SF Pro Display",-apple-system,BlinkMacSystemFont,Inter,"Helvetica Neue",sans-serif` and `"SF Mono",SFMono-Regular,ui-monospace,Menlo,monospace`. No webfont downloads. |
| 3 | Head metadata | PASS | Has doctype, `lang="en"`, charset utf-8, viewport, title "Terafab Command Center · a Snowflake demo concept", description, robots noindex/nofollow and the favicon. |
| 4 | HTTPS subpath serving | PASS | Served at `http://localhost:8941/terafab-demo/` (`python3 -m http.server`, subfolder). Tested at 1440 and 390: all 6 views, Watch steps 1–7, Replay, FEL/LPP toggle, Approve, an Assistant question, and View SQL opening. 0 console errors and 0 failed requests or 4xx. WebGL is active. Simulated no-WebGL: the page shows the fallback note, every view and the Assistant work, there are 0 page errors, and the only console lines are three.js's own expected "Error creating WebGL context". Everything is relative or inline, so the HTTPS subpath behaves the same. |
| 5 | Browsers | PASS | Chromium (Chrome engine) 1440/390 and WebKit (Safari engine) 1440/390 are both clean. Firefox is out of scope: it is not permitted in this environment. |
| 6 | Size / validity | PASS | `index.html` is 830,812 bytes (0.79 MB), far below GitHub's 100 MB file limit and 1 GB Pages site limit. A tag-balance parse found 0 unclosed or stray tags. No severe issues. |
| 7 | Storage / routing | PASS | The app never uses localStorage or sessionStorage. There's no hash routing: views switch with `data-go` click handlers, so there's nothing to conflict with Pages paths. |

## Publish with GitHub Pages
1. On GitHub: **New repository** (e.g. `terafab-command-center`). Pick its visibility (see the access note below).
2. Upload the *contents* of `deploy/` to the repo root on `main`: `index.html`, `.nojekyll`, `robots.txt`, `README.md` (optionally also this file). With git: `git init && git add . && git commit -m "Terafab demo" && git branch -M main && git remote add origin <repo-url> && git push -u origin main`.
3. **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main` / `/ (root)` → Save.**
4. Wait for the "pages build and deployment" action to finish (about 1 minute). The site will be at `https://<user-or-org>.github.io/<repo>/`, e.g. `https://<user>.github.io/terafab-command-center/`.
5. Open the URL and check Watch, Wafer map (3D) and the Assistant.

**Access note:** GitHub Pages on a free account requires a **public** repo, and the resulting site is public. Publishing Pages from a **private or internal** repo needs GitHub Pro, Team or Enterprise. Even then, the site itself is public unless you're on **GitHub Enterprise Cloud**, which offers access control: Pages visibility "Private", restricted to people with repo read access. Because Terafab/xAI marks are for internal and call use only, use a private repo with access-controlled Pages on Enterprise Cloud. `noindex` and `robots.txt` only discourage search engines; they don't control access.
