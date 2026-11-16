# HarperLinks

**Your business. Your AI workforce. One platform.**
A father & son build from Perth, WA.

An AI Business Operating System: a chat-first command centre where AI employees learn
your business from your website, write and schedule your content, capture leads, book
clash-checked appointments and run automations — plus the marketing site that sells it.

---

## Quick start

```bash
npm install
npm start          # http://localhost:4700
```

- `/` — the marketing site
- `/app` — the Business OS dashboard

### Live vs demo mode

| Mode | When | What you get |
|---|---|---|
| **DEMO** | no API key | A keyword brain runs the same tools — ingest, write, schedule, publish all work with canned-but-plausible content. |
| **LIVE** | `ANTHROPIC_API_KEY` set | Real Claude writes in the brand's voice and drives every tool conversationally. |

Copy `.env.example` to `.env` and add a key to go live. **Never commit `.env`.**

---

## What's in here

```
server.js          Express app — pages + the whole JSON API, port 4700
src/
  engine.js        Claude tool-use loop + the keyless demo brain
  prompt.js        System prompt, built from the business profile + knowledge base
  tools.js         Bot tools: ingest_website, create_post, schedule_post,
                   create_lead, book_appointment (clash-checked), add_task …
  ingest.js        URL → readable text → business profile
  scheduler.js     30s tick: publishes due posts, surfaces due tasks
  events.js        Automation engine — events fire enabled workflows
  employees.js     The six AI employees + their enabled/mode state
  datetime.js      Natural-language dates ("tomorrow 3pm", "next week")
  store.js         JSON-file store with atomic writes
public/
  index.html       The marketing site (all CSS/JS inline)
  app.html         Dashboard shell
  app.js           Core runtime: router, sidebar, shared widgets
  pages.js         Every dashboard page
  social.js        Shared AI chat builder + the Social Media page
tools/
  build-ghl-block.py   Regenerates ghl/PASTE-INTO-GHL.html from public/index.html
brand/             Logo pack (transparent, square, banner) + usage notes
ghl/               Paste-ready block for hosting the design inside GoHighLevel
site-export/       Deployable static copy of the marketing site
```

### Generated — don't hand-edit

`ghl/PASTE-INTO-GHL.html` and `site-export/index.html` are both built from
`public/index.html`. Edit the source, then regenerate:

```bash
python3 tools/build-ghl-block.py
```

Hand-editing the copies makes them silently drift from the real site.

---

## The AI employees

| | Role | Status |
|---|---|---|
| **Sage** | Social Media Manager | Works |
| **Riley** | Customer Support Agent | Works in chat; social DMs need Meta approval |
| **Harper** | AI Receptionist | Needs a Twilio number + voice connection |
| **Flynn** | Sales Agent | Works; auto-send needs email/SMS |
| **Indi** | Website Builder | Works; live CMS edits need WordPress access |
| **Darcy** | Business Manager | Works |

The switches are real, not decoration. Turn Sage off and the scheduler genuinely stops
publishing. Put Sage in **Assisted** mode and generated content is held as drafts for
approval instead of self-scheduling. Turn Flynn off and workflow lead-steps skip with a
logged reason.

---

## Honest status

This project does not fake anything, and shouldn't start:

- **Publishing runs in preview mode.** The scheduler fires exactly on time and logs every
  publish, but posts are not pushed to Instagram/Facebook/TikTok until those platform
  integrations are connected. `src/scheduler.js → publish()` is the single seam to swap in
  real API calls.
- **Video** — the bot writes real scripts and shot lists. Rendering footage needs a video
  AI integration.
- **Phone, email/SMS, payments** — not connected. Those pages show a designed setup state,
  never fake data.
- **Stats are always real.** Metrics that need an unconnected integration say
  "Not connected" rather than showing a made-up number.

Keep it that way.

---

## Deploying

**Marketing site** — static, no build. Drag `site-export/` onto Netlify, or use any
static host. See `site-export/README.md`.

**Dashboard** — needs a Node host (Railway, Render, Fly.io), because of the AI engine,
scheduler and datastore. It can't go on a static host.

**Inside GoHighLevel** — `ghl/README.md` walks through pasting the design into a GHL page
so their CRM/calendar/automations power it.
