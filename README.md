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

`vercel.json` redirects `www` to the bare domain. To prefer `www` instead, flip it in Vercel's
Domains settings and remove the redirect from `vercel.json`.
