# Deploy — GitHub Pages

This folder is a plain static site: no build step, no framework. Every page links `styles.css`
and `assets/`, so the whole folder must be published as-is from the repository root.

## One-time setup

1. Create a GitHub repository (e.g. `eisaal-website`) and push this folder as its root.
2. Repository → **Settings → Pages** → *Build and deployment* → Source: **Deploy from a branch** →
   Branch: `main`, folder: `/ (root)` → Save.
3. Wait for the first deploy (≈1 min). The site is live at `https://<owner>.github.io/eisaal-website/`.
   `.nojekyll` is present so GitHub serves the files verbatim.

## Custom domain — `eisaal.com` (decision d1)

DNS is on GoDaddy. Replace the dead records found on 2026-08-26 (apex A → `192.46.210.170`,
`www` CNAME → `eisaal-foundation.onrender.com`) with GitHub Pages' records:

| Host | Type | Value |
|---|---|---|
| `@` | A | `185.199.108.153` |
| `@` | A | `185.199.109.153` |
| `@` | A | `185.199.110.153` |
| `@` | A | `185.199.111.153` |
| `www` | CNAME | `<owner>.github.io` |

Then in **Settings → Pages → Custom domain** enter `eisaal.com`, wait for the DNS check, and tick
**Enforce HTTPS** once the certificate is issued (can take up to an hour). GitHub writes a `CNAME`
file into the repo at that point — commit it.

Do **not** add MX records here; email for `help@eisaal.com` is decision d3 and is configured
separately (GoDaddy forwarding or a mailbox provider).

## Before going live

- Fill every `[PLACEHOLDER: …]`: `grep -rn "PLACEHOLDER" *.html`. The `.ph` span styling makes
  unfilled ones visibly dashed on the live page — nothing should remain.
- Verify each URL below returns 200 over HTTPS, and the header/footer links resolve.

## URLs to paste into the stores

| Field | URL |
|---|---|
| App Store Connect → App Information → **Privacy Policy URL** | `https://eisaal.com/privacy-policy.html` |
| App Store Connect → App Information → **License Agreement / EULA** | `https://eisaal.com/terms.html` |
| App Store Connect → App Information → **Support URL** | `https://eisaal.com/support.html` |
| App Store Connect → App Information → **Marketing URL** (optional) | `https://eisaal.com/` |
| Play Console → Store listing → **Privacy policy** | `https://eisaal.com/privacy-policy.html` |
| Play Console → App content → Data safety → **Account deletion URL** | `https://eisaal.com/delete-account.html` |
| Play Console → Store listing → **Website** | `https://eisaal.com/support.html` |

Set the same values in the app's `PRIVACY_POLICY_URL`, `WEBSITE_URL` and `HELP_CENTER_URL`
dart-defines (GitHub Actions repository variables) so in-app links match — see
`docs/launch/RECON-AND-DECISIONS.md` § w1–w7 in the app repository.

## Download link — `https://eisaal.com/download`

The link to put in posters, QR codes, social posts and the app's Share sheet. The redirect runs in the
browser from the user agent; nothing is logged or sent anywhere, so it needs no privacy-policy change.

| Visit | Result |
|---|---|
| iPhone / iPad (incl. iPadOS in desktop mode) | App Store `id6746689559` |
| Android | Google Play `com.eisaal.com` |
| Desktop, or JavaScript off | The page: both badges + QR code |
| `?platform=ios` / `?platform=android` | Forces that store (e.g. a store-specific QR) |
| `?stay` | Shows the page without redirecting — use it to check the page on a phone |
| `?utm_source=…&utm_campaign=…` | Forwarded to Play as the install `referrer` (Play Console → acquisition reports) |

The store ids are written in `download.html` (head script and badge `href`s) and `index.html` (badge `href`s and the closing script). If the QR code ever
needs regenerating: `pip install segno`, then
`segno.make('https://eisaal.com/download', error='m').save('assets/download-qr.svg', border=0, dark='#2C2C2C', light=None, xmldecl=False, omitsize=True)`.

## Updating

Edit the HTML, push to `main`. Pages redeploys automatically. To preview locally:
`python3 -m http.server 8080` in this folder, then open `http://localhost:8080/`.
