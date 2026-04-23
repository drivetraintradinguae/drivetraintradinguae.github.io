# DriveTrain Trading — Website Operator's Guide

> **Audience:** whoever on the Drivetrain team is maintaining this site after launch. You don't need to be a web developer — if you can edit a Google Doc, you can update this website.

---

## What you have

Four files in one folder:

```
drivetrain-website/
├── index.html      ← the page itself
├── styles.css      ← colors, fonts, spacing
├── logo.png        ← the Drivetrain logo (displays in header + footer)
└── README.md       ← this guide
```

Everything lives in these files. No databases, no plugins, no subscriptions required.

---

## 1. Update the WhatsApp number, email, and city — in ONE place

Open `index.html`, scroll to the top of the `<body>`, and find this block:

```html
<script>
  window.DT_CONFIG = {
    whatsapp:        '971508776449',       // no + or spaces, international format
    whatsappDisplay: '+971 50 877 6449',   // how the number appears on screen
    email:           'info@drivetraintrading.com',
    city:            'Sharjah, United Arab Emirates'
  };
</script>
```

Change any of those four values and **the entire site updates on its own** — the floating WhatsApp button, the contact form submit, the header "channels" block, the footer, the quote form all pull from these four values at page load.

You never need to hunt through HTML to change a phone number again.

---

## 2. Put the site on the internet

### Option A — GitHub Pages (your existing setup)

1. Upload all four files (`index.html`, `styles.css`, `logo.png`, `README.md`) to your GitHub repo.
2. Repo → Settings → Pages → Source: `main` branch, root folder → Save.
3. Wait 30 seconds, site is live at `https://drivetraintradinguae.github.io/drivetrain-website/`.

### Option B — Netlify Drop (zero account)

1. Open [app.netlify.com/drop](https://app.netlify.com/drop).
2. Drag the whole folder onto the page.
3. Live in under 10 seconds.

---

## 3. Editing page content

Open `index.html` in any text editor. Find the section you want to edit using comment markers like:

```html
<!-- HERO -->
<!-- PARTS CATEGORIES — swipeable carousel on mobile -->
<!-- INVENTORY — swipeable on mobile -->
```

Edit the text between the `>` and `<` characters only. Don't touch tags, classes, or attributes unless you know what they do.

**To add a new inventory item:**
Find any `<article class="inv-card">` block in `index.html`, copy it start-to-end, paste below, change the title/meta/image class. Save. Done.

---

## 4. Adding real inventory photos

Right now the inventory tiles are colored gradients with labels. When you have real photos:

1. Create a folder called `images/` next to `index.html`.
2. Drop in compressed photos (under 500 KB — use [tinyjpg.com](https://tinyjpg.com)).
3. In `index.html`, find an inventory card's placeholder div:
   ```html
   <div class="inv-card__media inv-card__media--eng" role="img" aria-label="Used engine"><span>ENGINE</span></div>
   ```
4. Replace the entire `<div>` with:
   ```html
   <img src="images/engine-01.jpg" alt="Used Toyota 2.5L engine" class="inv-card__media" loading="lazy">
   ```
5. Save, upload, refresh. Google Images will eventually index them — that's free inbound traffic.

---

## 5. Installing marketing pixels (Meta, TikTok, Google)

**Best approach: Google Tag Manager (GTM).** Install once in the HTML, then add every pixel from GTM's web dashboard forever after.

1. Create account at [tagmanager.google.com](https://tagmanager.google.com) → create a Web container named `DriveTrain`.
2. GTM gives you two snippets — **Head** and **Body**.
3. Open `index.html`, find `<!-- START GTM HEAD -->` / `<!-- END GTM HEAD -->` and paste the Head snippet between them.
4. Find `<!-- START GTM BODY -->` / `<!-- END GTM BODY -->` and paste the Body snippet.
5. Save, upload, done forever on the HTML side.

**Then, from GTM's dashboard:** add Meta Pixel, TikTok Pixel, GA4, Google Ads — all without touching the site again.

Prefer direct-paste instead of GTM? The HTML already has marked blocks for each platform (`<!-- START META PIXEL -->` etc.) — paste the code they give you between the markers.

---

## 6. Language translation

The language switcher lives in the sticky header (so it's always visible, including on mobile). It offers **English, Русский, Українська, العربية, ქართული** — each language name always shown in its own script, regardless of which language the page is currently displaying.

- Selecting a language applies Google Translate site-wide and remembers the choice across page refreshes.
- Selecting English reverts cleanly.

**Known limitation:** machine translation isn't perfect. Once a market becomes important (say RU or AR traffic is >30% of visits), commission a human translation and publish as `index-ru.html` / `index-ar.html` with proper right-to-left layout for Arabic.

**To add more languages:** in `index.html`, find `includedLanguages: 'ru,uk,ar,ka'` and add language codes. Also add a matching `<li role="option" data-lang="xx">` in the language list, and an entry in the `LANG_LABELS` object at the bottom of the script.

---

## 7. Customizing colors and fonts

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

## 8. Replacing the logo

The logo is a single file: `logo.png`. Replace it with a new file of the same name — the header and footer both pick up the change automatically.

**Current setup note:** the logo you provided is designed for dark backgrounds (white gear + red DT on black). The header has a white background, so the CSS applies a filter that inverts the logo on the header only. In the footer (dark background), it displays as-is.

If you commission a dark-on-white version of the logo in future:
1. Replace `logo.png`
2. In `styles.css`, find `.brand__logo` and remove the `filter: invert(1) brightness(0.95);` line

---

## 9. Launch checklist

- [ ] `DT_CONFIG` at the top of `index.html` has your real WhatsApp, email, and city
- [ ] `logo.png` uploaded alongside `index.html`
- [ ] Tested "Request Quote" button on a phone — opens WhatsApp with the right number
- [ ] Floating WhatsApp button goes to the right number
- [ ] Tested language switcher — selecting Arabic / Russian / English works and English is always in the list
- [ ] Tested the swipe feature on mobile — Parts / Inventory / Why all scroll horizontally
- [ ] No horizontal scrolling of the whole page on mobile
- [ ] Installed GTM + GA4 so you can measure traffic from day one
- [ ] Submitted site to Google Search Console
- [ ] Added site URL to email signature, WhatsApp Business profile, business cards
- [ ] Added a `favicon.ico` (the logo serves as the icon automatically via `<link rel="icon" type="image/png" href="logo.png">`)

---

## 10. Known v1 limitations

Intentional. Name them so nothing is a surprise:

| Limitation | When to address |
|---|---|
| Inventory photos are gradient placeholders | Replace with real photos as you shoot them (top 6 first) |
| Inquiry-by-WhatsApp, no real checkout | When SKUs + prices stabilize, migrate to Shopify or WooCommerce |
| Google machine translation, not human | When a language hits >30% traffic, commission proper localization |
| One-page site | Split into `inventory.html` when stock volume >100 live items |
| No backend / database | Not needed — the site is brochureware plus a WhatsApp deep link |
| Sharjah map is illustrative, not geographic | Fine for a trust signal; if you need a real interactive map, use Google Maps embed (free) |

---

## 11. Debugging

- Site looks broken? Hard refresh (Ctrl+Shift+R / Cmd+Shift+R) to bypass cache.
- WhatsApp button goes to wrong number? Check `DT_CONFIG.whatsapp` at the top of `index.html` — international format, no `+`, no spaces.
- Language switcher doesn't show? Clear cookies and refresh. Google Translate persists your last choice.
- Logo displays as a broken image? Confirm `logo.png` is uploaded to the same folder as `index.html`.

---

**Update the inventory weekly. Reply to WhatsApp quickly. That's the whole job.** Everything else is static and maintains itself.
