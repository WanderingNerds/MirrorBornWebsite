# MirrorBornGames.com

A plain static site (HTML + CSS, no build step). Edit `index.html` and `styles.css`.

Preview locally: `npx serve .` (or just open `index.html`).

## Deploy to Vercel

1. Push this folder to a GitHub repo.
2. In Vercel: **Add New → Project → Import** the repo.
3. Framework Preset: **Other**. Leave Build Command and Output Directory empty. Click **Deploy**.
4. Every push to `main` redeploys automatically; other branches get preview URLs.

(Or from a terminal: `npm i -g vercel && vercel --prod`.)

## Point the domain from Cloudflare

1. In Vercel: **Project → Settings → Domains**, add `mirrorborngames.com` and `www.mirrorborngames.com`.
   Vercel shows the exact records to create; use those if they differ from below.
2. In Cloudflare: **mirrorborngames.com → DNS → Records**, add:

   | Type  | Name  | Content                | Proxy status |
   |-------|-------|------------------------|--------------|
   | A     | `@`   | `76.76.21.21`          | DNS only (grey cloud) |
   | CNAME | `www` | `cname.vercel-dns.com` | DNS only (grey cloud) |

   Delete any older A/AAAA/CNAME records on `@` or `www` that conflict.
3. Wait a few minutes. Vercel's Domains page turns green and issues the SSL cert automatically.

**Why DNS only?** Vercel handles SSL and its own CDN. Cloudflare's orange-cloud proxy can block
Vercel's certificate issuance and double-cache. If you do want the proxy on later, set Cloudflare
**SSL/TLS → Overview** to **Full (strict)** first, or you'll get redirect loops.

Choose which of `www` or the bare domain is primary in Vercel's **Settings → Domains**; Vercel
handles the redirect between them. Don't add a second redirect in `vercel.json`, or the two can loop.

## Game download

The Download buttons point to `https://downloads.mirrorborngames.com/GameIdea-client-latest.zip`
(search `GameIdea-client-latest` in `index.html` to change it). To make that URL work:

1. Cloudflare → **R2** → your bucket → **Settings → Custom Domains → Connect Domain**, enter
   `downloads.mirrorborngames.com`. Cloudflare creates the DNS record and certificate.
2. Upload each new build zip over the same name, e.g. from the GameIdea repo:
   `aws s3 cp build/GameIdea-client-<sha>.zip s3://<bucket>/GameIdea-client-latest.zip --endpoint-url https://<account_id>.r2.cloudflarestorage.com`
