# Zimyo Programs — campaigns, initiatives & report launches (static, no PHP)

A self-contained static site for Zimyo's campaigns, community initiatives and
flagship report launches. Plain HTML/CSS/JS — no WordPress, no PHP, no server.
Deploys to Render (or Netlify/Cloudflare Pages/any static host). Does NOT touch zimyo.com.

## Files
- `index.html`   — the **homepage** (hero, live program grid with filters, report-launch
  countdown, "Coming soon" states, impact numbers, launch playbook, subscribe CTA). Self-contained.
- `report.html`  — a **gated report** page (teaser → lead form → unlock), themed as
  *State of HR in India 2026*. Self-contained.
- `zimyo-logo.png` — official Zimyo logo (referenced by both pages).
- `robots.txt`   — allows crawl so search engines can read the noindex meta.
- `render.yaml`  — Render static-site config; also sends `X-Robots-Tag: noindex` (Blueprint deploys only).

## 1. Send leads somewhere (no backend needed)
Open `report.html`, find near the bottom of the `<script>`:

    var FORM_ENDPOINT = "";   // <-- paste your endpoint here

Create a free form at https://formspree.io (or Getform/Basin), copy its URL, e.g.:

    var FORM_ENDPOINT = "https://formspree.io/f/abcdwxyz";

The page POSTs `{name, email, company, size, report}` there over HTTPS. Free-email
filtering (blocks Gmail/Yahoo/etc.) and the on-page unlock are handled in the browser —
no server code required. Prefer HubSpot/Zoho? Use their web-to-lead endpoint URL instead.

The homepage subscribe form (`index.html`) currently shows an on-page confirmation only;
wire it to the same endpoint the same way if you want to capture those too.

## 2. Deploy on Render
1. Push this folder to a Git repo (already at `github.com/Aru0006/campaings`).
2. Render dashboard → **New → Static Site** → connect the repo.
3. **Build command:** (leave empty)   **Publish directory:** `.`
4. Deploy → you get `https://<name>.onrender.com`. Every push to `main` auto-redeploys.

## 3. Custom domain (DNS only)
Render → your site → **Settings → Custom Domains** → add e.g. `programs.zimyo.com`.
Render shows a CNAME target. At your DNS provider add ONE record:

    CNAME   programs   →   <name>.onrender.com

Render issues SSL automatically. zimyo.com is never modified.

## SEO
`report.html` carries `<meta name="robots" content="noindex,nofollow">`. If you deploy via
render.yaml (Blueprint), it also adds `X-Robots-Tag: noindex`. A subdomain is a separate SEO
entity, so there's zero impact on zimyo.com. To make a page indexable, remove its meta tag
(and the header). The homepage `index.html` is currently indexable — add the meta tag if you
want it hidden too.

## Editing
Plain HTML — edit text/numbers directly in `index.html` / `report.html`. To publish changes:

    git add -A
    git commit -m "Describe your change"
    git push
