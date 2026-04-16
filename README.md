# eigendeck.dev

Marketing site for [Eigendeck](https://github.com/dgleich/eigendeck), hosted on Cloudflare Pages.

## Local preview

```bash
python3 -m http.server 8000
# or
npx serve .
```

Open `http://localhost:8000`.

## Setup Guide

### 1. Create the GitHub repo (done)

```bash
cd website
gh repo create dgleich/eigendeck-web --public --source=. --push
```

Repo: https://github.com/dgleich/eigendeck-web

### 2. Connect to Cloudflare Pages

1. Go to [Cloudflare Dashboard](https://dash.cloudflare.com/) > **Workers & Pages** > **Create**
2. Select **Pages** > **Connect to Git**
3. Authorize GitHub and select the `dgleich/eigendeck-web` repo
4. Build settings:
   - **Framework preset**: None
   - **Build command**: _(leave empty)_
   - **Build output directory**: `/` (root — it's a static site)
5. Click **Save and Deploy**
6. Note the generated URL (e.g., `eigendeck-web.pages.dev`) — you'll need this for DNS

### 3. Set up Hover.com DNS → Cloudflare

For each domain (eigendeck.dev, eigendeck.com):

#### Option A: Keep DNS at Hover (simpler)

1. Log in to [hover.com](https://www.hover.com/)
2. Click the domain (e.g., `eigendeck.dev`) > **DNS** tab
3. Delete any existing A or CNAME records for `@` and `www`
4. Add these records:

   | Type | Host | Value |
   |------|------|-------|
   | CNAME | `@` | `eigendeck-web.pages.dev` |
   | CNAME | `www` | `eigendeck-web.pages.dev` |

   **Note:** Hover may not allow CNAME on the apex (`@`). If so, use Option B.

5. If Hover won't allow CNAME on `@`, use their "Forward" feature instead:
   - Set up `www` CNAME → `eigendeck-web.pages.dev`
   - Set up a forward from `eigendeck.dev` → `www.eigendeck.dev`

#### Option B: Transfer DNS to Cloudflare (recommended)

This gives you Cloudflare's CDN, SSL, and analytics for free.

1. **Add domain to Cloudflare:**
   - In Cloudflare Dashboard > **Websites** > **Add a site**
   - Enter `eigendeck.dev` > Select **Free** plan
   - Cloudflare will scan existing DNS records and show them

2. **Update nameservers at Hover:**
   - Cloudflare will show you two nameservers (e.g., `ada.ns.cloudflare.com`, `ben.ns.cloudflare.com`)
   - Go to Hover > domain > **Nameservers** tab
   - Change from Hover's defaults to the Cloudflare nameservers
   - Save. Propagation takes 1-24 hours (usually ~30 minutes)

3. **Add DNS records in Cloudflare:**
   - Once the domain is active in Cloudflare, go to **DNS** > **Records**
   - Add:

   | Type | Name | Content | Proxy |
   |------|------|---------|-------|
   | CNAME | `@` | `eigendeck-web.pages.dev` | Proxied |
   | CNAME | `www` | `eigendeck-web.pages.dev` | Proxied |

4. **Repeat for `eigendeck.com`** (same nameserver change at Hover, same DNS records in Cloudflare)

### 4. Add custom domains in Cloudflare Pages

After DNS is set up:

1. Go to **Workers & Pages** > **eigendeck-web** > **Custom domains** tab
2. Click **Set up a custom domain**
3. Add each domain one at a time:
   - `eigendeck.dev`
   - `www.eigendeck.dev`
   - `eigendeck.com`
   - `www.eigendeck.com`
4. Cloudflare will verify DNS and provision SSL certificates automatically
5. Wait for status to show **Active** (usually 1-5 minutes if DNS is on Cloudflare)

### 5. Verify

After setup, all of these should work:
- `https://eigendeck.dev` → site
- `https://www.eigendeck.dev` → redirects to `eigendeck.dev`
- `https://eigendeck.com` → site
- `https://www.eigendeck.com` → redirects to `eigendeck.com`

The `_redirects` file handles www → apex redirects:
```
https://www.eigendeck.dev/*  https://eigendeck.dev/:splat  301
https://www.eigendeck.com/*  https://eigendeck.com/:splat  301
```

### 6. SSL

Cloudflare handles SSL automatically. No action needed. Certificates are provisioned and renewed for all custom domains.

## Structure

```
index.html   — single-page site
style.css    — all styles
_redirects   — Cloudflare Pages www→apex redirects
README.md    — this file
```

## Updating

Push to `main`. Cloudflare Pages auto-deploys within ~30 seconds.

```bash
git add -A && git commit -m "Update site" && git push
```
