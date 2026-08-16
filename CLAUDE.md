# CLAUDE.md — altheastix-site

Public landing page for altheastix.com. This repo is a deployable artifact;
business knowledge, data, and strategy live in the private `altheastix-knowledge`
repo — never copy customer data, sales figures, or CRM material here.

## What this is
- One static page: `index.html` (all CSS/JS inline). No build step, no framework,
  no dependencies. Keep it that way unless Javier explicitly decides otherwise.
- `join/index.html` — redirect to `/#join`. This URL is printed on physical
  backing cards as a QR target. NEVER rename or remove this path.
- `assets/AltheastixLogo.png` — bolt-pattern logo (favicon + background texture).
- Deploys via GitHub Pages, custom domain altheastix.com (DNS on Cloudflare).

## Design system (do not drift)
- Palette: stage `#1B1226` · stage-2 `#2A1B3D` · poster cream `#FFF3DC` ·
  magenta `#F23D8A` · tangerine `#FF6B2C` · gold `#FFC53D` · violet `#8C5BD9` ·
  ink `#17101F`. All defined as CSS variables in `:root` — change tokens there,
  never hardcode colors inline.
- Type: Shrikhand (display, used sparingly) / Space Grotesk (body). Google Fonts.
- Aesthetic: psychedelic gig-poster. Signature element: the ticket-stub signup
  form. The crowd silhouette SVG closes the hero. Keep boldness concentrated
  there; everything else stays quiet.
- Quality floor: mobile-first (QR arrivals are phones), visible keyboard focus,
  `prefers-reduced-motion` respected.

## Voice & copy rules
- Warm, upbeat, playful, gratitude-forward, small-shop personal. "Hooray!",
  "our little shop", sign-off "The Altheastix Team", Seattle identity.
- Musician quotes: verified originals only. Spanish-language artists quoted in
  original Spanish + English translation.
- Marquee stat claims (units shipped, feedback %) must be sanity-checked against
  the live eBay storefront before any copy change, and rounded DOWN, never up.
  These are public claims about the business — understating is free, overstating
  is not. Last verified by Javier 2026-08-16: "50,000+ stickers shipped" and
  "99.3% positive feedback" are both accurate as published.
- HARD RULES (eBay policy): no discount language tied to buying off eBay, no
  price comparisons vs eBay, no "contact us to order direct" copy. The page
  invites newsletter signup and links to the eBay store. Nothing else until the
  Stripe Bestsellers phase is explicitly launched.

## Live TODOs
- Signup is wired to EmailOctopus via their inline embed script (form
  `3a20966a-99a4-11f1-8c1e-d12546154605`, list `5d27f50c-99a1-11f1-8b9a-ef86de0c49a1`).
  EmailOctopus retired the raw form-POST endpoint, so the script is the only
  supported route — Javier approved it 2026-08-16 as an explicit, one-off
  exception to the no-dependencies rule. It is the ONLY external script on the
  page; do not treat it as precedent for adding others. The ticket-stub chrome
  stays ours; only the email field and button come from EmailOctopus, styled to
  match there. Worth revisiting: `https://eomail5.com/form/<id>` answers 405 to
  GET, so a direct POST may be possible — that would let the script go away.
- Bestsellers/Stripe section: not built. When added, "setlist" card hrefs move
  from eBay links to Stripe Payment Links (bundles first).

## Workflow
- Preview locally by opening index.html (no server needed).
- Commit style: small, imperative subject lines.
- Show and push: run routine git operations (commit, pull, rebase, push)
  directly rather than handing Javier commands to paste. When a push changes
  the page, show him a summary of what changed and push in the same turn — the
  summary is a record, not a request for approval. Still confirm first for
  irreversible or outward-facing actions (force-push, history rewrite, sending
  a real newsletter), and leave anything needing his login to him.
