# Moving to picassodoodles.com on GoDaddy

Everything on GoDaddy. Nothing on Wix.

---

## First, the one thing that trips everyone up

"A website" is really **two separate purchases**, and they are easy to confuse:

| Piece | What it does | Where it lives |
|---|---|---|
| **The domain** | The name `picassodoodles.com` and its DNS records | GoDaddy — Domains |
| **The hosting** | The computer that actually serves `index.html` | GoDaddy — Web Hosting (cPanel) |

You need **both**, and they are billed separately.

### Which GoDaddy hosting product to buy

> **Buy "Web Hosting" (also called Linux Hosting / cPanel). Do NOT buy "Websites + Marketing" (Airo Websites).**

This matters. GoDaddy sells two very different things:

- **Websites + Marketing / Airo** is a drag-and-drop builder. It is GoDaddy's Wix equivalent. It **cannot host these files** — you can only paste small custom-code snippets into their template blocks, not upload a real `index.html`. If you buy this, you will be rebuilding the site in a builder, which is exactly what you're trying to get away from.
- **Web Hosting (cPanel)** gives you a `public_html` folder you upload files into. This is what this site needs. The cheapest tier ("Economy") is plenty — this is two static HTML files with no server-side code.

---

## Step 1 — Buy the domain

1. GoDaddy → search `picassodoodles.com` → buy it.
2. During checkout, **decline** the "add a website" / Websites + Marketing upsell. You want the domain only at this stage.
3. Turn **on** Domain Privacy if it's included. Your registration contact info is otherwise public in WHOIS, and this is a home-based business.

If `picassodoodles.com` is already taken, GoDaddy will say so at search time. Stop and reassess before buying hosting — the rest of this guide assumes you own it.

## Step 2 — Buy Web Hosting (cPanel)

1. GoDaddy → Hosting → **Web Hosting** → Economy (Linux).
2. During setup it asks which domain to attach. Choose `picassodoodles.com`.
3. Because the domain and hosting are both in your GoDaddy account, GoDaddy wires up the DNS **automatically**. You should not have to touch A records by hand.

Give it up to an hour. `picassodoodles.com` will start showing a GoDaddy placeholder page — that means DNS is working and it's ready for your files.

## Step 3 — Upload the site

1. GoDaddy → My Products → Web Hosting → **Manage** → **cPanel Admin** → **File Manager**.
2. Open the **`public_html`** folder. This is the web root — anything in here is publicly visible at your domain.
3. Delete GoDaddy's placeholder files if any exist (usually a `default.html` or a `cgi-bin` you can leave alone).
4. Upload these four files from this repo:
   - `index.html` → becomes `picassodoodles.com`
   - `admin.html` → becomes `picassodoodles.com/admin.html`
   - `.htaccess` → forces HTTPS and strips `www`
   - `robots.txt` and `sitemap.xml` → for Google

**About `.htaccess`:** it starts with a dot, so it's a hidden file. In File Manager click **Settings** (top right) and tick **"Show Hidden Files (dotfiles)"** or you will upload it and then not be able to see it.

## Step 4 — Turn on SSL (the padlock)

GoDaddy Web Hosting includes a free SSL certificate, but it is not always switched on by default.

1. cPanel → **SSL/TLS Status**.
2. Select `picassodoodles.com` and `www.picassodoodles.com` → **Run AutoSSL**.
3. Wait ~15 minutes, then load `https://picassodoodles.com`.

The `.htaccess` file redirects all `http://` traffic to `https://`, so **do not upload it until SSL is actually working** — otherwise you get a redirect loop into a certificate error. If that happens, rename `.htaccess` to `htaccess.txt` in File Manager, fix SSL, then rename it back.

---

## Step 5 — Firebase: the site will break without this

⚠️ **This is the single most important step. Skip it and the admin panel stops working.**

The site stores every puppy, dog, and inquiry in Firebase (project `picasso-doodles`), and `admin.html` logs you in by **texting a code to your phone**. Firebase refuses to send that code from any domain it doesn't recognize — this is a deliberate anti-abuse check, and a new domain is an unrecognized domain.

1. Go to the [Firebase Console](https://console.firebase.google.com/) → project **picasso-doodles**.
2. **Authentication** → **Settings** tab → **Authorized domains**.
3. **Add domain** → `picassodoodles.com`
4. **Add domain** → `www.picassodoodles.com`

Leave the existing entries alone (`localhost`, `picasso-doodles.firebaseapp.com`, and your old domain) until the new site is confirmed working — they cost nothing and they're your fallback.

**How to know it worked:** open `https://picassodoodles.com/admin.html` and request a login code. If the text arrives, you're done. If you get an error about an unauthorized domain or the reCAPTCHA fails to appear, the domain isn't saved correctly — check for a typo or a stray `https://` (enter the bare hostname only).

## Step 6 — Cloudinary photos

Photo and video uploads in the admin panel go to Cloudinary (cloud name `picassodogs`). These usually keep working with no changes.

If uploads fail after the move: Cloudinary → **Settings** → **Upload** → find your unsigned upload preset → check whether **Allowed origins** (or a referral-restriction setting under Security) has a domain whitelist on it. If it does, add `picassodoodles.com`. If it's empty, nothing to do.

---

## Step 7 — Point the old domain at the new one

Don't cancel `picassodoodlesworldwide.com`. Anyone with the old link, and every Google result you've built up, still points there. Keep it registered and **forward** it:

1. GoDaddy → My Products → the old domain → **Manage DNS** → **Forwarding**.
2. Forward to `https://picassodoodles.com`.
3. Set the type to **Permanent (301)**. This is the setting that tells Google "this moved for good, transfer the search ranking" — a temporary 302 does not.

Keep the forward running for at least a year. It's cheap insurance.

### If the old domain is currently on Wix

To get fully off Wix:

1. Set up the new site first and confirm it works. **Don't tear anything down until then.**
2. In Wix, **disconnect** `picassodoodlesworldwide.com` from the Wix site (Wix dashboard → Settings → Domains).
3. In GoDaddy, set that domain's nameservers back to GoDaddy's defaults if Wix changed them, then apply the 301 forward above.
4. **Then** cancel the Wix plan — in Wix's own billing, not GoDaddy's. Cancelling in the wrong order takes the old site down while the redirect isn't ready yet.

If the domain was *bought through* Wix rather than just pointed at it, you'll need to transfer the registration to GoDaddy first (Wix → Domains → Transfer Away, get the auth code, then GoDaddy → Transfer Domain). That takes 5–7 days, so start it early.

---

## Step 8 — Tell Google

1. [Google Search Console](https://search.google.com/search-console) → add `picassodoodles.com` as a new property.
2. Verify it — easiest method is the **HTML file** option: it gives you a small file to drop into `public_html` exactly like the site files.
3. Submit `https://picassodoodles.com/sitemap.xml`.
4. If you had the old domain in Search Console, use the **Change of Address** tool there to formally move it.

Also update the domain anywhere it's written down off-site: Instagram bio, Google Business Profile, Good Dog listing, Facebook, TikTok, YouTube.

---

## What changed in the code

- Every `picassodoodlesworldwide.com` in the Terms and Privacy Policy text is now `picassodoodles.com` (3 in `index.html`, 1 in `admin.html`).
- Added canonical + Open Graph tags to `index.html` pointing at `https://picassodoodles.com/`, so link previews and Google both resolve to the new domain instead of splitting between the two.
- Added `noindex` to `admin.html` so the admin panel stays out of search results.
- Added `.htaccess`, `robots.txt`, `sitemap.xml`.

**Deliberately left alone:**
- The Instagram handle `@picassodoodlesworldwide` — that's a live social account, not the domain. Changing the link would break it. Rename it on Instagram first if you want them to match.
- The contact email `picassodoodlesworldwide@gmail.com` — it's a Gmail address, unaffected by the domain change. If you'd like `hello@picassodoodles.com` instead, GoDaddy sells email separately, and you'd want to update it in `index.html` (3 places) and `admin.html` (1 place).
- The business name "Picasso Doodles Worldwide" in the policy headers — the *domain* shortened, the legal/business name is your call. Say the word and I'll change it.

## Publishing edits later

The files on GoDaddy are a **copy**. Editing them here in GitHub does not update the live site. Any time you change `index.html` or `admin.html`, re-upload the changed file through cPanel File Manager, overwriting the old one.

(Day-to-day content — puppies, dogs, inquiries — lives in Firebase, not in these files. Those edits go through the admin panel and appear instantly with no re-upload.)

---

## One unrelated thing I noticed

The "✨ Generate" AI buttons in the admin panel (`admin.html`, the `aiGen` function) call the Anthropic API with no API key attached, so they currently fail and show "Error. Try again." every time. This is pre-existing and unrelated to the domain move, so I left it alone. Worth knowing it's not the migration that broke it — and note that fixing it by pasting a key into `admin.html` would expose that key to anyone who views the page source, so it needs a small server-side piece instead. Happy to take that on separately.
