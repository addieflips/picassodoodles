# Moving to picassodoodles.com, all on GoDaddy, off Wix

## Where things stand

| | Domain | Currently at | Becomes |
|---|---|---|---|
| Keep | **picassodoodles.com** | **Wix** | The live site, at GoDaddy |
| Keep | **picassodoodlesworldwide.com** | GoDaddy | A permanent redirect to the above |
| Kill | The Wix site + plan | Wix | Gone |

Both domains are confirmed registered, so nothing needs buying except hosting.

The complication: **the domain you want to keep is the one stuck at Wix.** So this
isn't just a content move — the *registration* of `picassodoodles.com` has to be
transferred from Wix to GoDaddy, and that takes up to 7 days and can't be rushed.

> ## ⚠️ Read this before you touch anything
>
> **Do not cancel the Wix plan or account until the domain transfer has fully
> completed.** Wix is the current registrar for `picassodoodles.com`. Cancelling
> first can cost you the domain outright, and it is the one mistake in this whole
> process that isn't recoverable. Cancelling Wix is the *last* step, not the first.

The plan below gets your real site live on GoDaddy **first**, on the domain you
already control there, so nothing is ever down while the slow transfer runs in
the background.

---

## Step 0 — Check for the 60-day lock (do this today, it sets your timeline)

ICANN blocks domain transfers for 60 days after a domain is registered,
transferred, or has its contact info changed. If `picassodoodles.com` tripped any
of those recently, you simply cannot move it yet.

Wix account → **Domains** → `picassodoodles.com` → check the registration date and
whether it's locked.

- **Not locked** → carry on, the whole thing takes about a week.
- **Locked** → note the date it lifts. Everything in Steps 1–3 still proceeds
  normally in the meantime; only Step 4 waits.

Two other things to check while you're in there:

- **Was the domain free with your Wix Premium plan?** Free-with-plan domains
  often can't be transferred during the first year. If so, you may need to wait
  it out or pay to release it — Wix support can tell you which applies.
- **Is Private Registration on?** Turn it **off** before you start the transfer.
  The authorization code gets emailed to the registrant contact address, and
  privacy protection can hide or block that email.

---

## Step 1 — Buy GoDaddy Web Hosting

> **Buy "Web Hosting" (Linux / cPanel). Do NOT buy "Websites + Marketing" (Airo).**

This is the decision that matters most. Websites + Marketing is GoDaddy's
drag-and-drop builder — it's their Wix equivalent, and it **cannot host these
files**. It only lets you paste small snippets into their templates. Buying it
means rebuilding the site in another builder, which is the exact thing you're
trying to get away from.

Web Hosting gives you a `public_html` folder you upload real files into. The
cheapest **Economy** tier is plenty — this is two static HTML files.

When setup asks which domain to attach, choose **`picassodoodlesworldwide.com`**
for now. It's already in your GoDaddy account, so DNS wires up automatically and
you get a working site within the hour. `picassodoodles.com` comes later, once
it's actually yours at GoDaddy.

## Step 2 — Upload the site

1. GoDaddy → My Products → Web Hosting → **Manage** → **cPanel Admin** → **File Manager**
2. Open **`public_html`** — this is the web root
3. Delete GoDaddy's placeholder page if one is there (leave `cgi-bin` alone)
4. Upload from this repo:
   - `index.html` → the main site
   - `admin.html` → your admin panel
   - `.htaccess` → redirects and caching
   - `robots.txt`, `sitemap.xml` → for Google

**Turn on SSL before the `.htaccess` matters:** cPanel → **SSL/TLS Status** →
select the domain → **Run AutoSSL**. Wait ~15 min. The `.htaccess` forces HTTPS,
so if the certificate isn't ready you'll get a redirect loop into a cert warning.
If that happens, rename `.htaccess` to `htaccess.txt`, fix SSL, rename it back.

The `.htaccess` I wrote is **host-agnostic on purpose** — its HTTPS and www rules
work on whichever domain is attached, so it's safe right now on
`picassodoodlesworldwide.com`. The block that forces everything to
`picassodoodles.com` is commented out, with a warning. Leave it commented until
Step 5.

## Step 3 — Firebase (the site breaks without this)

⚠️ Your admin panel logs in by **texting a code to your phone** via Firebase.
Firebase refuses to send that code from a domain it doesn't recognize.

[Firebase Console](https://console.firebase.google.com/) → project
**picasso-doodles** → **Authentication** → **Settings** → **Authorized domains** →
**Add domain**. Add all of these now, so you never have to think about it again
as the domain flips:

- `picassodoodles.com`
- `www.picassodoodles.com`
- `picassodoodlesworldwide.com`
- `www.picassodoodlesworldwide.com`

Leave the existing entries (`localhost`, `picasso-doodles.firebaseapp.com`) alone.

**Test it:** load `/admin.html` on the live site and request a login code. If the
text arrives, you're good. If reCAPTCHA fails or you see an unauthorized-domain
error, there's a typo — enter the bare hostname only, no `https://`.

**Cloudinary** (photo uploads, cloud name `picassodogs`) usually needs nothing. If
uploads fail: Cloudinary → Settings → Upload → your unsigned preset → check
whether **Allowed origins** has a whitelist. If it's empty, nothing to do.

> **At this point your real site is live on GoDaddy and fully working.** Everything
> below is about moving the name across. Take your time with it.

---

## Step 4 — Transfer picassodoodles.com from Wix to GoDaddy

This is the slow part. Up to 7 days.

**At Wix:**
1. Wix account → **Domains**
2. Click the **Domain Actions** icon next to `picassodoodles.com`
3. **Transfer away from Wix** → **I Still Want to Transfer**
4. Make sure the domain shows as **unlocked**
5. Wix emails a **transfer authorization code** (also called an EPP or auth code)
   to your registrant contact address — check spam if it doesn't arrive

**At GoDaddy:**
6. GoDaddy → **Domains** → **Transfer a domain** → enter `picassodoodles.com`
7. Paste the authorization code
8. Pay the transfer fee (~$10–20 — it includes a year's renewal, so it's not
   wasted money)
9. Approve the confirmation email GoDaddy sends

Then wait. It can complete in a couple of days or take the full week. GoDaddy
shows the status under Domains the whole time.

**During the wait, change nothing.** The Wix site stays up on `picassodoodles.com`,
your new site stays up on `picassodoodlesworldwide.com`, both work, no downtime.

## Step 5 — Switch the domains over

Once GoDaddy shows `picassodoodles.com` in your account:

1. **Attach it to the hosting.** cPanel → **Domains** → add `picassodoodles.com`
   and point it at the same `public_html`. If your Economy plan won't allow a
   second domain, change the account's **primary domain** to `picassodoodles.com`
   instead (GoDaddy support will do this in a minute if the option isn't obvious).
2. **Re-run AutoSSL** for the new domain — cPanel → SSL/TLS Status. Don't skip
   this or the site loads with a certificate warning.
3. **Confirm `https://picassodoodles.com` serves your site**, not Wix's.
4. **Now** turn on the canonical redirect: edit `.htaccess` and uncomment the two
   lines in the `PHASE 3 ONLY` block. That permanently 301s
   `picassodoodlesworldwide.com` → `picassodoodles.com`.

If cPanel won't attach the old domain as an alias, do the redirect at the
registrar instead: GoDaddy → the old domain → **Manage DNS** → **Forwarding** →
forward to `https://picassodoodles.com`, type **Permanent (301)**. The 301 is what
tells Google to move your search ranking; a 302 doesn't.

**Keep `picassodoodlesworldwide.com` registered** for at least a year. Every old
link, business card, and Google result still points at it. It's cheap insurance.

## Step 6 — Now cancel Wix

Only once `https://picassodoodles.com` is confirmed serving your GoDaddy site:

1. Wix → **Settings** → **Subscriptions** — cancel the Premium plan
2. Confirm `picassodoodles.com` no longer appears under Wix Domains (it shouldn't,
   post-transfer)
3. Delete the Wix site, and close the account if you want

Download anything you still want off the Wix site first — photos, testimonials,
copy. Once it's deleted it's gone.

## Step 7 — Tell Google

1. [Search Console](https://search.google.com/search-console) → add
   `picassodoodles.com` as a property → verify with the **HTML file** method (drop
   the file into `public_html` like the others)
2. Submit `https://picassodoodles.com/sitemap.xml`
3. If `picassodoodlesworldwide.com` is in Search Console, use the **Change of
   Address** tool to formally move it
4. Update the domain off-site: Instagram bio, Google Business Profile, Good Dog,
   Facebook, TikTok, YouTube

---

## The short version

| # | Do | Wait |
|---|---|---|
| 0 | Check the 60-day lock at Wix | — |
| 1 | Buy GoDaddy **Web Hosting (cPanel)**, attach old domain | ~1 hr |
| 2 | Upload files, run AutoSSL | ~15 min |
| 3 | Add all 4 domains to Firebase authorized domains | — |
| 4 | Start Wix → GoDaddy transfer of picassodoodles.com | **up to 7 days** |
| 5 | Attach new domain, re-run SSL, uncomment redirect block | ~1 hr |
| 6 | **Then** cancel Wix | — |
| 7 | Search Console + update social links | — |

---

## What changed in the code

- Every `picassodoodlesworldwide.com` in the Terms and Privacy Policy copy is now
  `picassodoodles.com` (3 in `index.html`, 1 in `admin.html`)
- Canonical + Open Graph tags in `index.html` pointing at
  `https://picassodoodles.com/`, so previews and search settle on the new domain
- `noindex` on `admin.html` to keep the admin panel out of search results
- `.htaccess`, `robots.txt`, `sitemap.xml`

The canonical tag names `picassodoodles.com` while the site is temporarily served
at the old domain. That's intentional — it's the correct end state, it's harmless
for a week, and it means nothing to remember to change later.

**Deliberately unchanged:**
- Instagram handle `@picassodoodlesworldwide` — a live account, not the domain.
  Changing the link would break it. Rename it on Instagram first if you want them
  to match.
- Contact email `picassodoodlesworldwide@gmail.com` — a Gmail address, unaffected.
  If you want `hello@picassodoodles.com`, that's a separate GoDaddy purchase plus
  a code change in 4 places.
- The business name "Picasso Doodles Worldwide" in the policy headers — the
  *domain* shortened; whether the *brand* does is your call.

## Publishing edits later

The files on GoDaddy are a **copy**. Editing them in GitHub does not update the
live site — re-upload the changed file through cPanel File Manager.

Day-to-day content (puppies, dogs, inquiries) lives in Firebase, not in these
files. Those edits go through the admin panel and appear instantly.

## One unrelated bug

The "✨ Generate" AI buttons in the admin panel (`aiGen` in `admin.html`) call the
Anthropic API with no API key attached, so they fail every time with "Error. Try
again." Pre-existing and unrelated to the move — flagging so you know the
migration didn't cause it. Note that fixing it by pasting a key into `admin.html`
would expose that key to anyone viewing page source, so it needs a small
server-side piece instead.
