# Remixa site

The public site for Remixa, served from GitHub Pages as one unit:

- `index.html` — landing page (hero, features, pricing, how-it-works, downloads, footer)
- `guide.html` — full walkthrough guide
- `version.json` — feed the RepurposeStudio auto-updater reads to offer updates
- `images/`, `videos/`, `logo.png` — assets

**Live URL:** https://remixa.app/

---

## 1. Checkout + downloads — LIVE state

### Checkout (Lemon Squeezy overlay)
Both pricing-card buttons ($99/mo monthly card, $990/yr yearly card) are wired
to the Lemon Squeezy overlay: `lemon.js` is loaded at the bottom of
`index.html`, the buttons carry `class="lemonsqueezy-button"` and share ONE
product checkout link — `?enabled=1914469` limits the monthly card to the
monthly variant, `?enabled=1914446` the yearly card to the yearly variant, and
`?embed=1` keeps the buyer on remixa.app.

The checkout link UUID (`2d78f483-acf3-4ed9-a77a-d0dfa5ab383e`) is the
product's share-link UUID from the LS dashboard (live mode) → Store →
Products → Remixa → **Share**. If the product is ever re-created, copy the
new share link and update both hrefs in `index.html`.

### Downloads ("Already bought?" section)
Points at **https://app.lemonsqueezy.com/my-orders** — the stable page where
every buyer logs in with their purchase email (magic link) and always gets the
newest installers. Lemon Squeezy file URLs are per-customer and signed, so
there is no public direct installer URL to link.

### Discord invite
Both footers link to https://discord.gg/sd3PyBAkKV (`index.html` and
`guide.html`). Nothing to do unless the invite changes.

---

## 2. Shipping a new version (the 2-minute routine)

When you build a new installer:

1. Upload the new Windows `.exe` and Mac `.zip` to **both variants** of the
   Lemon Squeezy product (replace the old files). Existing customers
   automatically get the new files on their My Orders page — that is the
   download path; nothing on the site needs touching for it.
2. Edit **`version.json`** — bump `"version"` (must go **up**, e.g. `"2.9.7"`
   → `"2.9.8"`) and write one line in `"notes"`.
3. Commit + push. GitHub Pages redeploys in ~1 min.

**Updater caveat (as of 2.9.7):** `windows.url` / `mac.url` in `version.json`
are intentionally **empty**, so the in-app update banner never appears and
customers update by re-downloading from My Orders. The app's updater only
accepts a stable, public, direct `.exe`/`.dmg`/`.pkg` URL (anything else errors
at download time), and Lemon Squeezy doesn't provide one — its download links
are per-customer and signed. To turn the in-app banner on for a future
release: host the installers at a stable direct URL (e.g. a GitHub Release —
see section 3) and put those URLs in `version.json`.

Validate before pushing: `python -m json.tool version.json` — it must parse
clean (no trailing commas).

---

## 3. Important: installers are NOT hosted here

The installers are ~156 MB. **GitHub Pages rejects any file over 100 MB**, so
they cannot live in this repo or at remixa.app. They are uploaded to the Lemon
Squeezy product (both variants) and delivered through checkout receipts and My
Orders. If a stable public installer URL is ever wanted (to make the in-app
update banner work), use a GitHub Release on this repo — assets up to 2 GB,
stable direct URLs — but note that makes the raw installer publicly
downloadable (the app still requires a license key to run), which is an owner
call. remixa.app only serves the HTML, images, and `version.json`.

---

## 4. remixa.app custom-domain checklist

DNS is already configured at Namecheap. To finish the cutover:

**In GitHub → repo → Settings → Pages:**
1. Under "Custom domain", enter `remixa.app` and Save. This creates/commits a
   `CNAME` file in the repo — leave it there.
2. Wait for the "DNS check successful" green check (can take a few minutes to an
   hour while it verifies the Namecheap records).
3. Tick **"Enforce HTTPS"** once the certificate is issued (may take up to ~24h
   after the DNS check passes).
4. Confirm https://remixa.app/ and https://remixa.app/guide.html both load.

**Then switch the three `og:`/social URLs in EACH HTML file** from `github.io`
to `remixa.app` (there's a comment block at the top of each `<head>` with the
exact target values):

`index.html`
- `og:url` → `https://remixa.app/`
- `og:image` → `https://remixa.app/images/social-card-home.png`
- `twitter:image` → `https://remixa.app/images/social-card-home.png`

`guide.html`
- `og:url` → `https://remixa.app/guide.html`
- `og:image` → `https://remixa.app/images/social-card.png`
- `twitter:image` → `https://remixa.app/images/social-card.png`

*(Everything else uses relative `./` paths and needs no change.)*
