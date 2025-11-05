# Deployment Guide

This repository is configured for two hosting targets:

- cPanel/Apache (e.g., cyberkeris.com on node303.vpsmalaysia.com.my)
- Netlify (optional)

The app is a React + Vite SPA. The production build is output to `dist/client`.

## 1) Build locally

```pwsh
cd "c:\Users\Alan The Marvel\Downloads\G-Vet-Aset-and-iStor-1\G-Vet-Aset-and-iStor-1"
npm ci
npm run build
```

Artifacts: `dist/client` (contains index.html, assets, .htaccess, premium_dashboard.html).

## 2) Deploy to cPanel (Apache)

You can upload via cPanel File Manager or FTP.

### a) cPanel File Manager (recommended)

1. Login to cPanel (URL from your host).
2. Open File Manager → `public_html`.
3. Upload the contents of `dist/client` (or upload a zip and Extract).
4. Enable "Show Hidden Files" and confirm `.htaccess` is present in `public_html`.
5. Browse the site.

### b) FTP (manual)

- Server: your server IP or domain (e.g., 203.99.149.232 or cyberkeris.com)
- Username/Password: from your hosting provider
- Upload local `dist/client` to remote `/public_html`.

Tip: Use WinSCP (GUI) or FileZilla for easier drag-and-drop uploads.

## 3) Optional — GitHub Actions FTP deploy

A workflow is included at `.github/workflows/deploy.yml`.

Set these repo secrets (Settings → Secrets and variables → Actions → New repository secret):

- `FTP_SERVER`: 203.99.149.232
- `FTP_USERNAME`: your cPanel username
- `FTP_PASSWORD`: your cPanel password

On push to `main`, GitHub will build and deploy `dist/client` to `/public_html`.

## 4) Custom domains

### cyberkeris.com

- Update nameservers at your registrar to:
  - ns1.vpsmalaysia.com.my
  - ns2.vpsmalaysia.com.my
- After propagation, ensure your site lives under `public_html`.

### themarvel.space (addon or alias)

- In cPanel → Domains → Create a New Domain → enter `themarvel.space`.
- Set Document Root to `public_html/themarvel.space` (uncheck "Share document root").
- Upload a copy of `dist/client` into that folder.
- Update themarvel.space DNS at its registrar to either:
  - Change nameservers to the same as above; or
  - Point an A record to `203.99.149.232`.
- Issue SSL via cPanel’s AutoSSL (SSL/TLS Status → Run AutoSSL).

## 5) Netlify (optional)

- `netlify.toml` is already configured.
- Connect the repo on Netlify, set build command to `npm run build` and publish dir to `dist/client`.
- Add a custom domain in Netlify and update DNS (CNAME) accordingly.

## 6) Verify

- Root: `https://your-domain/`
- SPA routes: e.g., `/kewpa`, `/assets` should not 404 thanks to `.htaccess`.
- Premium page: `https://your-domain/premium_dashboard.html`.

If deep links 404 on Apache, ensure `.htaccess` exists and AllowOverride is enabled. Contact your host if needed.
