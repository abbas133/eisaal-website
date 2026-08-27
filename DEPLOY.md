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

## Updating

Edit the HTML, push to `main`. Pages redeploys automatically. To preview locally:
`python3 -m http.server 8080` in this folder, then open `http://localhost:8080/`.
