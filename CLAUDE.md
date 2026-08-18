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
- Palette ("Backstage", adopted 2026-08-17; desaturated the same day at Javier's
  request — the first pass read too vivid):
  stage `#121217` · stage-2 `#1B1B22` · bone `#E9E3D5` · oxblood `#7C3239` ·
  brass `#B29B62` · rust `#A9714E` · smoke `#9A93A6` · smoke-2 `#8E8899` ·
  ink `#050506` · ink-soft `#4A4550` · ink-faint `#5F5A67`. All defined as CSS
  variables in `:root` — change tokens there, never hardcode colors inline.
- `--stage` is deliberately a few steps lighter than `--ink`: the crowd
  silhouette is filled with `--ink`, and at closer values it disappears.
- Type: Alfa Slab One (display, used sparingly) / Space Grotesk (body). Google
  Fonts.
- Aesthetic: dark rock-poster / backstage. Dive bar, road cases, aged paper.
  Warm in tone despite the dark palette — the voice stays friendly, so the
  surface must not read cold or hostile.
- Signature element: the **all-access laminate** signup — bone card, oxblood
  edge stripe, punched lanyard slot. (It was a ticket stub with perforations
  before the 2026-08-17 redesign; the ticket/setlist/encore language stays.)
  The crowd silhouette SVG closes the hero. Keep boldness concentrated there;
  everything else stays quiet.
- The wordmark is SOLID bone with a maroon offset shadow. It had a
  gold-to-rust gradient in the first pass; that was the loudest element on the
  page and was deliberately removed. Do not reintroduce a gradient fill.
- Offset "poster" shadows on dark backgrounds must use brass, not ink — an ink
  shadow on a near-black page is invisible.
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
- Staging must never point at the production EmailOctopus form. The embed is
  gated on hostname in index.html — it loads only on altheastix.com /
  www.altheastix.com, and every other host gets an inert placeholder plus a
  noindex meta. Keep that gate host-based, never branch-based: a
  branch-diverged index.html can merge to main and silently kill signup. If
  you ever need to exercise a real submit on staging, add a throwaway
  EmailOctopus form + list and point the staging host at that instead.
- Bestsellers/Stripe section: not built. When added, "setlist" card hrefs move
  from eBay links to Stripe Payment Links (bundles first).

## Workflow
- Preview over a local server, not file:// — `python3 -m http.server 8765`,
  then localhost:8765. Root-relative paths (`/join`, `/#join`) and the
  hostname check on the signup embed don't behave correctly under file://.
- Commit style: small, imperative subject lines.
- Branch first. Design and content changes go on a feature branch and get
  previewed (locally, or on the Cloudflare Pages branch URL) before merging
  to main. main auto-deploys to altheastix.com — treat a push to main as a
  release, not a save.
- Show and push: run routine git operations (commit, pull, rebase, push)
  directly rather than handing Javier commands to paste. When a merge to main
  changes the page, show him a summary of what changed and push in the same
  turn — the summary is a record, not a request for approval. Still confirm first for
  irreversible or outward-facing actions (force-push, history rewrite, sending
  a real newsletter), and leave anything needing his login to him.
