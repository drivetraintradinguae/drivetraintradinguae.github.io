# DriveTrain Trading — Website Operator's Guide

> **Audience:** whoever on the Drivetrain team is maintaining this site. You don't need to be a web developer — if you can edit a Google Doc, you can update this website.

---

## What you have

Five files in one folder:

```
drivetrain-website/
├── index.html         ← the page itself
├── styles.css         ← colors, fonts, spacing
├── logo.png           ← horizontal long-form logo (header + footer)
├── logo-square.png    ← square logo (favicon + social share image)
└── README.md          ← this guide
```

Everything lives here. No databases, no plugins, no subscriptions.

---

## 1. Update contact info — all in ONE place

Open `index.html`, scroll to the top of the `<body>`, and find:

```html
<script>
  window.DT_CONFIG = {
    whatsapp:        '971508776449',                    // no + or spaces
    whatsappDisplay: '+971 50 877 6449',
    email:           'drivetraintradinguae@gmail.com',
    instagram:       'drivetrainuae',                   // handle, no @
    city:            'Sharjah, United Arab Emirates'
  };
</script>
```

Change any value here and the **entire site updates on its own** — floating WhatsApp button, footer links, contact section, form submission, Instagram link, all pull from these five values.

---

## 2. Hosting & privacy — the honest answer

You asked: *"I don't want the code to be public to protect from hacking and replicating."* Here's the real picture.

### What you can and can't hide

| Thing | Can you hide it? |
|---|---|
| HTML / CSS / JS in a visitor's browser | **No.** Every website on the internet exposes these — right-click → View Source works everywhere. This is how the web works. |
| The GitHub repository on github.com | **Yes.** Switch to a private repo + hosting that supports it. |
| Real warehouse photos, WhatsApp responsiveness, UAE license, Google ranking | **These are your moat.** They can't be replicated by copying code. |

### What "hacking" actually means here

Your site has: no backend, no database, no admin login, no user accounts, no payment processing, no API keys. There is almost nothing to attack. The only real risk is someone gaining access to your GitHub account and editing the files. That's prevented by:

- A strong, unique GitHub password
- Two-factor authentication (enable at github.com → Settings → Password and authentication)
- Not sharing repo access with people you don't trust

### Recommended: move to Netlify (free, private repo, code hidden from github.com)

This is what I'd do in your position. Free. Takes 15 minutes.

1. **Make your GitHub repo private:**
   - Go to your repo → Settings → scroll to bottom → *Change repository visibility* → Make private.
2. **Sign up at [netlify.com](https://netlify.com)** (free account, GitHub login).
3. **Netlify dashboard → Add new site → Import an existing project → GitHub → authorize.**
4. Pick your `drivetrain-website` repo. Build settings: leave empty (this is a static site). Click **Deploy**.
5. In ~60 seconds Netlify gives you a URL like `drivetrain-trading.netlify.app`.
6. **Settings → Domain management** → attach your real domain when you have one.
7. **Turn off GitHub Pages** on the old repo (Settings → Pages → None) so the github.io URL stops serving.

After that: your repo is private on GitHub (invisible to the public), Netlify deploys the site publicly from that private repo, and every time you edit files and commit, Netlify redeploys automatically.

- Code visible to site *visitors*: unavoidable (like every website ever).
- Code visible to *GitHub browsers*: now hidden.

### Alternative: Cloudflare Pages

Same workflow as Netlify, also free, also deploys from a private GitHub repo. Slightly better global performance but Netlify is friendlier for non-developers. Pick whichever.

### What NOT to do

- **Don't** pay for GitHub Pro just to keep the repo private. Netlify/Cloudflare Pages already solve this for free.
- **Don't** assume "code obfuscation" or "minification" actually protects anything. It doesn't. Anyone determined can un-minify in seconds.
- **Don't** store anything secret in the site files. API keys, passwords, customer data — they belong in a real backend, not here.

---

## 3. Putting the site live (first time)

### Via Netlify (recommended — supports private repos)

See section 2 above.

### Via GitHub Pages (public repo, free, matches current setup)

1. Push all five files to your GitHub repo.
2. Settings → Pages → Source: `main` branch, root folder → Save.
3. Live at `https://drivetraintradinguae.github.io/drivetrain-website/`.

---

## 4. Editing page content

Open `index.html` in a text editor. Find sections via comment markers like:

```html
<!-- HERO -->
<!-- PARTS CATEGORIES — swipeable carousel on mobile -->
<!-- INVENTORY — swipeable on mobile -->
```

Edit only the text between `>` and `<`. Don't touch tags, classes, or attributes.

**To add a new inventory item:** find any `<article class="inv-card">`, copy start-to-end, paste below, change the text. Save.

---

## 5. Adding real inventory photos

Currently the inventory tiles are colored gradients with labels. When you have real photos:

1. Create an `images/` folder next to `index.html`.
2. Drop in compressed photos (under 500 KB — use [tinyjpg.com](https://tinyjpg.com)).
3. In `index.html`, find an inventory card's placeholder div:
   ```html
   <div class="inv-card__media inv-card__media--eng" role="img" aria-label="Used engine"><span>ENGINE</span></div>
   ```
4. Replace with:
   ```html
   <img src="images/engine-01.jpg" alt="Used Toyota 2.5L engine" class="inv-card__media" loading="lazy">
   ```
5. Save, redeploy, refresh.

---

## 6. Installing pixels (Meta, TikTok, Google)

**Best approach: Google Tag Manager (GTM).** Install once in HTML, manage every pixel forever from GTM's dashboard.

1. Create Web container at [tagmanager.google.com](https://tagmanager.google.com).
2. GTM provides a **Head** snippet and a **Body** snippet.
3. In `index.html`, find `<!-- START GTM HEAD -->` / `<!-- END GTM HEAD -->` — paste Head snippet between them.
4. Find `<!-- START GTM BODY -->` / `<!-- END GTM BODY -->` — paste Body snippet.
5. Save, redeploy.

From GTM's dashboard: add Meta Pixel, TikTok Pixel, GA4, Google Ads — all without touching HTML again.

Direct-paste alternative: the HTML has marked blocks for each platform — paste the platform's snippet between the markers.

---

## 7. Language translation

Language switcher lives in the sticky header (always visible, including on mobile). Offers **English, Русский, Українська, العربية, ქართული** — each name always shown in its own native script.

- Selecting a language applies Google Translate site-wide and remembers the choice across refreshes.
- Selecting English reverts cleanly.

**Known limit:** machine translation isn't perfect. Once a market hits >30% of traffic, commission human-translated pages (`index-ru.html`, `index-ar.html` with proper RTL layout for Arabic).

---

## 8. Customizing colors and fonts

Open `styles.css`. The first block:

```css
:root {
  --red:        #b91c1c;
  --ink:        #0f172a;
  ...
}
```

Change hex codes → entire site re-colors. No search-and-replace anywhere else.

---

## 9. Replacing the logo

Two logo files:
- **`logo.png`** — horizontal long-form logo displayed in the header and footer
- **`logo-square.png`** — compact square version used as favicon and for social-share previews

Replace either file with a new version of the same name and the same aspect ratio — the site picks up the change automatically.

The header is dark by design so the white-on-dark logo sits naturally without any filter hacks. If you commission a new logo, keep the white-on-dark (negative) version — it's what the design expects.

---

## 10. Launch checklist

- [ ] `DT_CONFIG` at the top of `index.html` has your real WhatsApp, email, Instagram, city
- [ ] `logo.png` + `logo-square.png` uploaded alongside `index.html`
- [ ] Test Request Quote button on phone → opens WhatsApp with right number
- [ ] Floating WhatsApp button → right number
- [ ] Instagram channel card in contact section links to @drivetrainuae
- [ ] Language switcher: test English ↔ Russian ↔ Arabic, English always in dropdown
- [ ] Mobile: Parts / Inventory / Why sections scroll horizontally with swipe
- [ ] No horizontal page scroll on mobile
- [ ] GitHub account has 2FA enabled (critical)
- [ ] Repo set to private (if migrating to Netlify)
- [ ] Netlify deployment active (if using Netlify)
- [ ] GitHub Pages disabled on old repo (if migrated)
- [ ] GTM + GA4 active from day one so you can measure traffic
- [ ] Site submitted to [Google Search Console](https://search.google.com/search-console)
- [ ] Instagram @drivetrainuae has the site URL in its bio
- [ ] WhatsApp Business profile has the site URL

---

## 11. Known v1 limitations

| Limitation | When to address |
|---|---|
| Inventory tiles are gradients, not real photos | Replace with real photos as shot (top 6 first) |
| Inquiry-by-WhatsApp, no real checkout | When SKUs + prices stabilize, migrate to Shopify or WooCommerce |
| Google machine translation, not human | When one language hits >30% of traffic |
| One-page site | Split into separate `inventory.html` when live stock >100 items |
| No backend | Intentional — removes 100% of the attack surface |
| Regional map is illustrative, not geographic | Fine as a trust signal; swap for Google Maps embed if you need real geography |

---

## 12. Debugging

- **Site looks broken?** Hard refresh (Ctrl+Shift+R / Cmd+Shift+R) to bypass cache.
- **WhatsApp button goes to wrong number?** Check `DT_CONFIG.whatsapp` — international format, no `+`, no spaces.
- **Logo broken?** Confirm `logo.png` is in the same folder as `index.html`.
- **Language picker doesn't switch?** Clear cookies and refresh.
- **Netlify not redeploying?** Check the Deploys tab for build errors.

---

**Update inventory weekly. Reply to WhatsApp within the hour. That's the whole job.** Everything else is static and maintains itself.
