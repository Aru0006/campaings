# Zimyo Reports — reports.zimyo.com (static, no PHP)

A self-contained gated report page. Static HTML only — no WordPress, no PHP, no server.
Deploys to Render (or Netlify/Cloudflare Pages/any static host). Does NOT touch zimyo.com.

## Files
- `index.html`  — the whole gated report (teaser → form → unlock). Self-contained.
- `robots.txt`  — allows crawl so Google can read the noindex meta.
- `render.yaml` — Render static-site config; also sends `X-Robots-Tag: noindex`.

## 1. Set where leads go (no backend needed)
Open `index.html`, find near the bottom:

    var FORM_ENDPOINT = "";   // <-- paste your endpoint here

Create a free form at https://formspree.io (or Getform/Basin), copy its URL, e.g.:

    var FORM_ENDPOINT = "https://formspree.io/f/abcdwxyz";

The page POSTs {name, email} there over HTTPS. Business/free-email filtering and the
on-page unlock are already handled in the browser — no server code required.
(Prefer HubSpot/Zoho? Use their web-to-lead endpoint URL instead.)

## 2. Deploy on Render
1. Push this folder to a Git repo (GitHub/GitLab), OR use Render's manual deploy.
2. Render dashboard → New → **Static Site** → connect the repo.
3. Build command: (leave empty)   Publish directory: `.`
4. Deploy → you get `https://<name>.onrender.com`.

## 3. Point the subdomain (only DNS change; AWS untouched)
Render → your site → **Settings → Custom Domains** → add `reports.zimyo.com`.
Render shows a CNAME target. At your DNS provider add ONE record:

    CNAME   reports   →   <name>.onrender.com

Render issues SSL automatically. Live in minutes. zimyo.com is never modified.

## SEO
`index.html` has `<meta name="robots" content="noindex,nofollow">` and render.yaml adds
`X-Robots-Tag: noindex`. A subdomain is a separate SEO entity, so there is zero impact
on zimyo.com. (To make it indexable later, remove the meta tag + the header.)

## Editing the report
This is plain HTML (not Elementor). Text/numbers can be edited directly in `index.html`,
or ask to regenerate it from the build script for bigger changes.
