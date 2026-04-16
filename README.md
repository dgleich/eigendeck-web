# eigendeck.dev

Marketing site for [Eigendeck](https://github.com/dgleich/eigendeck), hosted on Cloudflare Pages.

## Local preview

```bash
# Any static server works
python3 -m http.server 8000
# or
npx serve .
```

Open `http://localhost:8000`.

## Deploying to Cloudflare Pages

### 1. Create the GitHub repo

```bash
cd website
gh repo create dgleich/eigendeck-web --public --source=. --push
```

### 2. Connect to Cloudflare Pages

1. Go to [Cloudflare Dashboard](https://dash.cloudflare.com/) > **Workers & Pages** > **Create**
2. Select **Pages** > **Connect to Git**
3. Authorize GitHub and select the `dgleich/eigendeck-web` repo
4. Build settings:
   - **Framework preset**: None
   - **Build command**: _(leave empty)_
   - **Build output directory**: `/` (root — it's a static site)
5. Click **Save and Deploy**

### 3. Custom domains

After the first deploy:

1. Go to **Workers & Pages** > **eigendeck-dev** > **Custom domains**
2. Add `eigendeck.dev`:
   - If the domain's DNS is on Cloudflare: it adds a CNAME automatically
   - If external DNS: add a CNAME record pointing to `eigendeck-dev.pages.dev`
3. Add `www.eigendeck.dev` the same way (Cloudflare will handle redirect)
4. Repeat for `eigendeck.com` and `www.eigendeck.com` if you own that domain

DNS records (if managing DNS elsewhere):

```
CNAME  eigendeck.dev      eigendeck-dev.pages.dev
CNAME  www.eigendeck.dev  eigendeck-dev.pages.dev
CNAME  eigendeck.com      eigendeck-dev.pages.dev
CNAME  www.eigendeck.com  eigendeck-dev.pages.dev
```

### 4. Redirects (optional)

Create `_redirects` to redirect www to apex:

```
https://www.eigendeck.dev/*  https://eigendeck.dev/:splat  301
https://www.eigendeck.com/*  https://eigendeck.com/:splat  301
```

## Structure

```
index.html   — single-page site
style.css    — all styles
_redirects   — Cloudflare Pages redirects (optional)
```

## Updating

Push to `main`. Cloudflare Pages auto-deploys.
