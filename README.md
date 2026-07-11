# Remixa site

The public site for Remixa, served from GitHub Pages as one unit:

- `index.html` — landing page (hero, features, pricing, how-it-works, downloads, footer)
- `guide.html` — full walkthrough guide
- `version.json` — feed the RepurposeStudio auto-updater reads to offer updates
- `images/`, `videos/`, `logo.png` — assets

**Live URL:** https://remixa.app/

---

## 1. Placeholders to fill in

Search-and-replace these exact tokens. Nothing else in the files needs editing.

### Checkout URLs (2× — same file)
Your Lemon Squeezy / payment checkout links.

| What | File | Line | Token |
|------|------|------|-------|
| Monthly ($99/mo) Buy button | `index.html` | ~386 | `PLACEHOLDER_CHECKOUT_URL` |
| Yearly ($999/yr) Buy button | `index.html` | ~393 | `PLACEHOLDER_CHECKOUT_URL` |

> Both tokens are identical text but are **two different links** — the monthly
> one goes on the monthly card, the yearly one on the yearly card. Replace each
> in place (don't use replace-all with one URL).

### Installer URLs (2× each in TWO files = 4 spots)
The Windows `.exe` and Mac `.dmg`. **These must be Lemon Squeezy download URLs,
not github.io or remixa.app** — see the warning in section 4.

| What | File | Line | Token |
|------|------|------|-------|
| Windows button | `index.html` | ~466 | `PLACEHOLDER_WIN_URL` |
| Mac button | `index.html` | ~468 | `PLACEHOLDER_MAC_URL` |
| Windows updater feed | `version.json` | 4 | `PLACEHOLDER_WIN_URL` |
| Mac updater feed | `version.json` | 5 | `PLACEHOLDER_MAC_URL` |

> Keep the two `PLACEHOLDER_WIN_URL` spots pointing at the **same** file, and
> likewise for Mac. The download button and the auto-updater must serve the
> identical installer.

### Discord invite
Already filled in — both footers link to https://discord.gg/XFpapzP2HC
(`index.html` and `guide.html`). Nothing to do unless the invite changes.

*(Line numbers above are approximate — search the token if the file has shifted.)*

---

## 2. Shipping a new version (the 2-minute routine)

When you build a new installer:

1. Upload the new Windows `.exe` and Mac `.dmg` to Lemon Squeezy. Copy their
   download URLs.
2. Edit **`version.json`**:
   - Bump `"version"` (e.g. `"2.8"` → `"2.9"`). This is what triggers the
     in-app "update available" prompt, so it must go **up**.
   - Write one line in `"notes"` — shown to users in the update prompt.
   - Paste the new `windows.url` and `mac.url` (only changes if the download
     URL changed; if the URL is stable you only touch version + notes).
3. If the download URLs changed, also update the two buttons in
   **`index.html`** (`PLACEHOLDER_WIN_URL` / `PLACEHOLDER_MAC_URL` spots).
4. Commit + push. GitHub Pages redeploys in ~1 min. The app polls `version.json`
   and offers the update.

Validate before pushing: paste `version.json` into <https://jsonlint.com> or run
`python -m json.tool version.json` — it must parse clean (no trailing commas).

---

## 3. Important: installers are NOT hosted here

The installers are ~156 MB. **GitHub Pages rejects any file over 100 MB**, so
they cannot live in this repo or at remixa.app. Host them on **Lemon Squeezy**
and point `PLACEHOLDER_WIN_URL` / `PLACEHOLDER_MAC_URL` (in both `index.html`
and `version.json`) at those Lemon Squeezy URLs. remixa.app only serves the
HTML, images, and `version.json`.

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
