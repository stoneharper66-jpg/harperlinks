# HarperLinks — website export

A self-contained copy of the public marketing site. No build step, no dependencies —
just static files. Exported 16 Nov 2026 from `~/harperlinks-automation/public`.

```
index.html        the whole marketing site (CSS + JS are inline)
assets/           logo-alpha.png (transparent), logo-glow.png (social share), favicon
app/index.html    placeholder for /app — see "About the dashboard" below
404.html          branded not-found page
_headers          security + cache headers (Netlify)
netlify.toml      publish config (no build command)
robots.txt
```

## Preview it locally

```bash
cd ~/harperlinks-site-export
python3 -m http.server 4800
```
Then open <http://localhost:4800>. Use a server rather than opening `index.html`
directly — `file://` breaks the root-relative links.

## Deploy it

**Netlify (drag & drop):** drag this folder onto <https://app.netlify.com/drop>. Done.

**Netlify CLI:**
```bash
cd ~/harperlinks-site-export
netlify deploy --prod --dir .
```

Works the same on Vercel, Cloudflare Pages or GitHub Pages — publish the folder as-is.

## About the dashboard

The marketing site is fully static, but the **Business OS dashboard is not** — it runs on
a Node/Express server with the AI engine, scheduler and JSON datastore behind it, so it
can't be hosted as static files.

Every "Launch App" button points at `/app`. Rather than let those 404, this export
includes `app/index.html`, which adapts to where it's being viewed:

- **Browsing locally** and the dashboard is running on `:4700` → it shows
  **"Your dashboard is running"** with an *Open the dashboard* button straight through to it.
- **Locally but the server is off** → it tells you to run `npm start`.
- **On a public domain** → it stays a plain "private beta" page. No localhost links are
  ever shown to real visitors.

To put the real dashboard online, deploy the Node app (`~/harperlinks-automation`) to a
host like Railway, Render or Fly.io, then point the `/app` links in `index.html` at that URL.

## Before going live

- **Socials** — the footer icons are inactive until you add real URLs. Find the `SOCIALS`
  array near the top of the `<script>` in `index.html` and fill in each `url`.
- **Contact** — there's no contact form on this page yet; add one if you want enquiries.
- **Pricing** — A$249 / A$499 / A$999 are shown as "free while we build". Keep that honest
  until Stripe is actually connected.
