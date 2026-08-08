# Deploying the Tablestock site

Three files, zero backend, free hosting. Recommended path: GitHub Pages, since you're already on GitHub for the app repo.

## 1. Push the site
Create a new repo (e.g. `tablestock-www`) and push `index.html`, `privacy.html`, `terms.html`, `style.css` to the root of the `main` branch. Keep this separate from the app's repo — a marketing/legal site and a mobile app codebase don't need to share history.

## 2. Turn on Pages
Repo → Settings → Pages → Source: "Deploy from a branch" → Branch: `main`, folder: `/ (root)`. Save. GitHub will build and give you a temporary `https://<username>.github.io/tablestock-www/` URL — confirm it renders before touching DNS.

## 3. Point the custom domain
Same Pages settings screen → Custom domain → enter `tablestock.app` → Save. GitHub will commit a `CNAME` file (just the text `tablestock.app`) into your repo automatically.

## 4. DNS records in Spaceship
In the Spaceship Advanced DNS panel for tablestock.app, add four A records on the apex/root (`@`):

```
A   @   185.199.108.153
A   @   185.199.109.153
A   @   185.199.110.153
A   @   185.199.111.153
```

Before adding these: check whether an A record already exists on `@`. If Spaceship put a parking-page A record there when you bought the domain, delete it first — otherwise GitHub's records won't take effect cleanly.

These are **A records on the root only** — they control where `tablestock.app` resolves for web traffic (port 80/443). They don't touch MX, DKIM, SPF, or DMARC, which are separate record types that route mail. Nothing about your Google Workspace email setup changes.

Optional: if you want `www.tablestock.app` to also resolve, add
```
CNAME   www   <username>.github.io
```

## 5. Verify + enforce HTTPS
DNS propagation is usually under an hour, sometimes up to 24. Once GitHub shows the custom domain as verified (green checkmark in Pages settings), tick "Enforce HTTPS." GitHub issues the TLS certificate automatically — no separate cert purchase needed.

## Before going live
- Replace the `[bracketed placeholders]` in `privacy.html` and `terms.html` (entity name + address) once the LLC is filed.
- Get Patrick's pass on the Terms, especially §7 (food-safety disclaimer) and §12 (governing law), before this is publicly linked anywhere real.
- Delete the two `<!-- DRAFT ... -->` HTML comments at the top of privacy.html/terms.html once cleared — cosmetic only, not visible on the page, but good hygiene.
