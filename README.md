# altheastix-site

Landing page for altheastix.com — gig-poster design, ticket-stub email signup.

## Structure
- `index.html` — the whole page (CSS/JS inline, no build step)
- `join/index.html` — redirect target for the QR code (`altheastix.com/join` → `/#join`)
- `assets/AltheastixLogo.png` — bolt-pattern logo, used as favicon + background texture

## Deploy (GitHub Pages)
```bash
cd ~/Documents/Claude/Projects/altheastix-site
git init && git add -A && git commit -m "Launch landing page"
gh repo create altheastix-site --public --source=. --push
```
Then in the repo: **Settings → Pages** → Deploy from branch → `main` / root.

**Custom domain:** Settings → Pages → Custom domain → `altheastix.com`.
In Cloudflare DNS add (all DNS-only/grey-cloud at first, proxy later if wanted):
- `A` records for `altheastix.com` → the four GitHub Pages IPs (only the third
  octet changes; all four end in `.153`):
  - `185.199.108.153`
  - `185.199.109.153`
  - `185.199.110.153`
  - `185.199.111.153`
- `CNAME` `www` → `ellokojavi.github.io` (the *user* subdomain — not the repo
  name, not `altheastix.com`)

Enable **Enforce HTTPS** once the certificate shows up (~15 min).

## Signup form
Wired to EmailOctopus — form `3a20966a-…`, list `5d27f50c-…`. The embed script
renders the email field and button; the ticket chrome around it is ours, and
the field/button styling is set in the EmailOctopus dashboard to match. It
loads on the production host only — see Staging below.

## Staging / previewing designs
Production is GitHub Pages on altheastix.com. Cloudflare Pages is wired to the
same repo for **previews only** — it never serves the real domain, so nothing
here can touch the production certificate.

Local:
```bash
git checkout -b my-redesign
python3 -m http.server 8765   # then localhost:8765
```
Use a server, not file:// — `/join` and `/#join` are root-relative.

Cloudflare Pages is already set up (project `altheastix-site`). Nothing to do
per-branch: pushing a branch triggers a build, and the preview lands at
`<branch>.altheastix-site.pages.dev`. A branch pushed *before* the project
existed won't have built — push any commit to it, or trigger it from
Deployments → Create deployment.

If you ever recreate the project: the dashboard's **Create application**
button only offers Workers on this account, and Workers Builds expects a
`wrangler` config this repo doesn't have. Reach the Pages flow directly at
`dash.cloudflare.com/<account-id>/pages/new/provider/github` — framework preset
**None**, build command **empty**, build output directory `/`, production
branch `main`. Never attach a custom domain to the Pages project; that would
pull the apex off GitHub Pages and invalidate the certificate.

**The signup form is gated by hostname.** The EmailOctopus embed loads only on
`altheastix.com` / `www.altheastix.com`. Any other host — localhost, any
pages.dev preview — gets an inert placeholder and a `noindex` meta, so no
preview can write a real contact to the live list or fire the welcome
automation. This is deliberately host-based rather than branch-based: a
branch-diverged index.html can merge to main and silently break signup.
`_headers` adds `X-Robots-Tag: noindex` on Cloudflare Pages as a second layer
(GitHub Pages ignores that file).

**Always test `/join` on a preview URL** — it's the printed QR target and the
one path that cannot break.

## Customizing
- Colors and fonts are all CSS variables at the top of `index.html` (`:root`).
- Footer quote rotates whenever you feel like it — keep it verified originals.
- The "setlist" cards all point at the eBay store for now; swap hrefs to
  Stripe Payment Links when the Bestsellers phase goes live.
