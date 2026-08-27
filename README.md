# eisaal-website

Public pages for the Eisaal app — home, privacy policy, terms of service, account deletion, and
support. Static HTML, published with GitHub Pages at `https://eisaal.com` (see `DEPLOY.md`).

## Layout

| File | Purpose |
|---|---|
| `index.html` | Home — what the app is, store links, legal/support index |
| `privacy-policy.html` | Store-listed privacy policy |
| `terms.html` | Terms of service / EULA |
| `delete-account.html` | Public account-deletion instructions (required by Play Data safety) |
| `support.html` | Contact + FAQ |
| `404.html` | GitHub Pages not-found page |
| `styles.css` | Single shared stylesheet — Sanctuary design system, Everyday · Light |
| `assets/` | App icon, Islamic stroke icons, geometry texture |

## Style rules

The site mirrors the app's Sanctuary theme so it reads as the same product. Tokens in
`styles.css` (`:root`) are copied from the app's `sanctuary_colors.dart` / `app_fonts.dart`:
Cinzel for eyebrows and section labels, Playfair Display for headings, Inter for body; warm-gold
`primaryBrand` for rims and accents, `primaryText` (#8B7420) for any gold text; the four-level
depth system as `--elevated` / `--raised` / `--floating` surfaces with their shadows and gold
edge luminance. One Tier-1 surface per page (the home hero); everything else is Tier-2 cards.

- Do not add colours, radii or spacing outside the `:root` tokens.
- Keep `[PLACEHOLDER: …]` text inside `<span class="ph">` until it is filled.
- Fonts load from Google Fonts with system fallbacks; no other external requests.

## Source of truth for wording

Claims about data collection must match `docs/launch/DATA-SAFETY-DECLARATION.md` in the app
repository; feature claims must match the shipped V1 scope (Ja'fari method, three prayer windows,
bundled content, no community features).
