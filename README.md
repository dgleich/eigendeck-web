# eigendeck.dev

Marketing site for [Eigendeck](https://github.com/dgleich/eigendeck), hosted on Cloudflare Pages.

Repo: https://github.com/dgleich/eigendeck-web

## Local preview

```bash
python3 -m http.server 8000
```

## Manual (`/manual`, `/learning`)

The manual pages under `manual/` and `learning/` are **generated** from the
markdown source in the main repo (`dgleich/eigendeck`, `docs/manual/`). Don't
hand-edit them here. To rebuild after editing the source:

```bash
# in a checkout of dgleich/eigendeck, with this repo cloned alongside as ./website
node tools/build-manual.mjs
cd website && git add -A && git commit -m "manual: rebuild" && git push
```

`manual.css` (the manual/learning styling) *is* maintained here.

## Setup (completed)

### Cloudflare Pages

1. Connected `dgleich/eigendeck-web` repo to Cloudflare Pages
2. Build settings: no framework, build command `exit 0`, output directory `/`
3. Auto-deploys on push to `main`

### eigendeck.dev (primary domain)

1. Added `eigendeck.dev` as a site in Cloudflare (free plan)
2. Changed nameservers at Hover to Cloudflare's nameservers
3. DNS records in Cloudflare:
   - CNAME `@` → `eigendeck-web.pages.dev` (Proxied)
   - CNAME `www` → `eigendeck-web.pages.dev` (Proxied)
4. Added `eigendeck.dev` and `www.eigendeck.dev` as custom domains in Pages

### eigendeck.com (redirect)

- Forwarded at Hover: `eigendeck.com` → `https://eigendeck.dev` (301 permanent)
- No Cloudflare setup needed

### Result

- `https://eigendeck.dev` → site
- `https://www.eigendeck.dev` → redirects to eigendeck.dev
- `https://eigendeck.com` → redirects to eigendeck.dev

## Updating

```bash
git add -A && git commit -m "Update site" && git push
```

Cloudflare auto-deploys within ~30 seconds.
