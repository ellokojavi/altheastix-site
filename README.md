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

## Wire up the signup form (one TODO in index.html)
1. In EmailOctopus: **Lists → your list → Forms → Embedded form**.
2. Copy the form's `action` URL and the email field's `name` attribute.
3. In `index.html`, find the `<!-- TODO: wire to EmailOctopus -->` comment,
   replace `FORM_ACTION_URL` and (if different) the `name="field_0"` value.
4. Commit + push. Done.

## Customizing
- Colors and fonts are all CSS variables at the top of `index.html` (`:root`).
- Footer quote rotates whenever you feel like it — keep it verified originals.
- The "setlist" cards all point at the eBay store for now; swap hrefs to
  Stripe Payment Links when the Bestsellers phase goes live.
