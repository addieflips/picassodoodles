# Moving to picassodoodles.com, and shutting down Wix

## Where things stand

| | What | Currently at | Becomes |
|---|---|---|---|
| Domain | **picassodoodles.com** | **Wix** (registrar) | Registered at GoDaddy, pointed at Netlify |
| Domain | **picassodoodlesworldwide.com** | GoDaddy (registrar) | Permanent redirect to the above |
| Hosting | The site files | **Netlify** | Unchanged — stays on Netlify |
| Kill | Wix site + plan | Wix | Gone |

**Your site is hosted on Netlify, not Wix and not GoDaddy.** The Firebase
authorized-domains list includes `peppy-licorice-348690.netlify.app`, which is a
Netlify address. Netlify is almost certainly wired to this GitHub repo and
republishes the site automatically every time you save a change here.

That's a good setup and worth keeping: it's free, SSL is automatic and
self-renewing, and you never upload a file by hand. So **no GoDaddy hosting
purchase is needed** — this whole job is a domain move, not a hosting move.

> ### You already own picassodoodles.com — do not try to buy it
>
> It's registered to you through Wix. You are **moving** it to GoDaddy, not
> purchasing it. If you search for it on GoDaddy it will show as unavailable
> (because it's yours), and GoDaddy may offer a **backorder**, **domain broker**,
> or **"make an offer"** service. Don't use any of those — they're for buying a
> domain off a stranger, they cost real money, and they can't get you a domain you
> already hold. The correct path is **Domains → Transfer a domain** (Step 2).

> ### ⚠️ Do not cancel Wix until the transfer finishes
>
> Wix is the current registrar for `picassodoodles.com`. Cancelling the plan or
> account first can forfeit the domain, and it's the one mistake here that isn't
> recoverable. Cancelling Wix is the **last** step.

---

## Step 1 — Finish the Firebase list (2 minutes, do it now)

Your admin panel logs in by texting a code to your phone via Firebase, and
Firebase won't send that code from a domain it doesn't recognize.

You've already added the two `worldwide` entries. Two are still missing:

- `picassodoodles.com`
- `www.picassodoodles.com`

[Firebase Console](https://console.firebase.google.com/) → project
**picasso-doodles** → **Authentication** → **Settings** → **Authorized domains** →
**Add domain**.

A "hostname" here is just the domain text — type it in by hand, bare, with no
`https://`. Nothing is fetched from Wix, GoDaddy, or Netlify for this step.

**Leave everything already in the list alone**, including the `.netlify.app` entry
— that's your live site's own address, and removing it breaks admin login
immediately. Adding these two now means the login keeps working straight through
the domain switch with no gap.

## Step 2 — Transfer picassodoodles.com from Wix to GoDaddy

The slow part. Up to 7 days, and it can't be rushed.

**First, check the 60-day lock.** ICANN blocks transfers for 60 days after a
domain is registered, transferred, or has its contact info edited. Wix →
**Domains** → `picassodoodles.com` → check the registration date and lock status.
If it's locked, note when it lifts; nothing else in this guide is blocked by it.

Two more things while you're there:
- **Was the domain free with your Wix Premium plan?** Free-with-plan domains often
  can't transfer during the first year. Wix support can tell you.
- **Turn off Private Registration.** The authorization code is emailed to the
  registrant address, and privacy protection can hide or block it.

> #### ⚠️ Don't touch "Edit contact info"
>
> It sits right above "Transfer away from Wix" in the Domain Actions menu, and
> it's the one item that can cost you a week. Changing the registrant name, email,
> or organization starts a **fresh 60-day ICANN transfer lock** — even if the
> domain is transferable today. Leave contact info exactly as it is until the
> transfer has completed; fix it at GoDaddy afterwards if it needs fixing.
>
> Toggling Private Registration off is a different setting and is safe.
>
> Also leave **"Unassign from this site"** alone. It disconnects the domain from
> the Wix site, taking `picassodoodles.com` down early for no benefit. The
> transfer doesn't need it, and Step 5 removes the Wix site anyway.

### Check MX records before you transfer

If any email address uses `@picassodoodles.com`, the transfer plus the DNS change
in Step 3 will break it unless you carry the MX records across.

Wix → Domain Actions → **Edit MX records**. Screenshot whatever is there.

- **Empty, or Wix defaults you don't use** → nothing to do.
- **Real mail records** (Google Workspace, Outlook, etc.) → re-create the same MX
  records at GoDaddy in Step 3, alongside the A and CNAME.

Your contact address is a plain Gmail (`picassodoodlesworldwide@gmail.com`), so
this is probably empty — but it's a ten-second check against a problem that's
miserable to debug later.

### If Wix blocks you: "paid for with a payment method that doesn't belong to you"

Wix refuses the transfer when the card paying for the domain belongs to someone
else, and offers an **Update payment method** link. That link is the fix — but
work out *whose card it is* before you click, because Wix warns that the previous
payment method's owner "won't have full access to the domain" afterwards.

**Whose card is it?**

- **Your own old or expired card, or a spouse's / family member's** → just update
  it to a card you own and carry on. Nothing to worry about.
- **A web designer, developer, agency, or friend who built the original site** →
  talk to them first. Not because you need their permission to own your own
  domain, but because they may have other things billed to that account, and a
  surprise loss of access is a bad way for them to find out. A two-line message
  saves a relationship.

**Paying is not the same as owning.** The registered owner of a domain is the
**registrant contact**, not whoever's card is attached. So updating the payment
method doesn't take the domain from anyone — it moves the billing to you and
restores your ability to act on a domain you already hold. If there's genuine
disagreement about who owns `picassodoodles.com`, sort that out with the person
directly; Wix's billing screen is not the place to settle it.

**Good news on timing:** changing the payment method is *not* a registrant contact
change, so it does **not** start a fresh 60-day transfer lock. Update the card and
you can request the transfer straight away.

**If you can't update it** — the card owner is unreachable, or the account is
locked to them — contact Wix support directly. Proving you're the registrant is a
route they can act on. Whatever happens, **do not let the domain expire** hoping
to re-register it at GoDaddy: expired domains get picked up by squatters within
hours, and you would not get it back.

**At Wix:**
1. Wix account → **Domains**
2. **Domain Actions** icon next to `picassodoodles.com` → **Transfer away from Wix**
3. **I Still Want to Transfer**, and make sure the domain shows **unlocked**
4. Wix emails a **transfer authorization code** (also called an EPP or auth code)
   — check spam

> #### The authorization code is a credential with a deadline
>
> Treat it like a password — it's the key that moves your domain. Don't paste it
> anywhere but GoDaddy's transfer form, and don't forward the email on. If it does
> leak before you use it, cancel the transfer request at Wix and request a new
> code; that invalidates the old one.
>
> **It expires in 7 days.** Miss that window and you start Step 2 over from the
> beginning — not fatal, just annoying. Use it the same day you get it.
>
> **Copy and paste it, never retype it.** These codes are case-sensitive and full
> of symbols that are easy to get wrong by eye — `O` vs `0`, `l` vs `1`. Watch for
> a trailing space sneaking in when you copy, which is the most common reason a
> valid code gets rejected.

**At GoDaddy:**
5. **Domains** → **Transfer a domain** → enter `picassodoodles.com`
6. Paste the authorization code
7. Pay the transfer fee (~$10–20 — it includes a year's renewal, so it's not lost
   money)
8. **Approve the confirmation emails.** GoDaddy sends one to the registrant
   address, and Wix usually sends its own "confirm transfer away" message.
   Approving both immediately is the difference between the transfer landing in
   a day or two versus sitting out the full 5-day auto-approval window. Check
   spam — this is the step that most often stalls, and it stalls silently.

#### If GoDaddy errors on the domain name, before asking for a code

"There was an error processing your request. Please try again." on the screen
where you type `picassodoodles.com` is a **different failure** from a rejected
code — GoDaddy hasn't asked for the code yet, so the code isn't involved. The
message is generic and tells you nothing, so work through it cheapest-first:

1. **Retry in a private/incognito window**, or a different browser. GoDaddy's
   transfer page is genuinely flaky and breaks on stale sessions, ad blockers,
   and privacy extensions. Costs 30 seconds and fixes this more often than it
   should.

2. **Check the lock at Wix.** Wix → Domains → `picassodoodles.com`. Requesting the
   authorization code and *unlocking* the domain are separate actions, and
   GoDaddy checks transferability the moment you enter the name. Unlock it, wait
   a few minutes, retry.

3. **Check for a transfer already in progress.** If an earlier attempt half-
   succeeded, GoDaddy → Domains may already list `picassodoodles.com` as pending.
   Starting a second transfer on top of a pending one errors out. Continue the
   existing one rather than starting over.

4. **Ask GoDaddy support.** They can see the actual registry response, which is
   the thing this error is hiding from you. Their chat is quick, and this is a
   two-minute question for someone who can read the log. Worth doing early rather
   than guessing — especially since the authorization code expires in 7 days and
   burning days on trial and error means starting Step 2 again.

Your authorization code stays valid while you sort this out. If it does lapse,
request a new one at Wix — that's routine, not a setback.

#### If GoDaddy rejects the authorization code

Work down this list — the first two cause most failures.

1. **Is the domain actually unlocked at Wix?** Requesting the code and unlocking
   the domain are *separate* things, and a locked domain fails no matter how
   correct the code is. Wix → Domains → `picassodoodles.com` → confirm it reads
   unlocked. This is the single most common cause.

2. **Does your code contain `<`, `>`, `&`, or `"`?** Many web forms silently strip
   these as a security measure, so a correct code arrives at the server mangled
   and gets rejected — with no indication that anything was altered. You cannot
   work around it by escaping or retyping. Request a **new code** from Wix
   instead; they're randomly generated, and the replacement most likely won't
   contain an awkward character. Requesting a new code invalidates the old one,
   which is fine.

3. **Trailing whitespace.** Double-clicking or triple-clicking to select often
   grabs a trailing space or newline. Paste into a plain text editor first, check
   the length, then copy again.

4. **Case.** These codes are case-sensitive. Autocorrect and phone keyboards like
   to capitalise a leading letter.

5. **The 60-day lock**, if none of the above apply. Blocked if the domain was
   registered or transferred within 60 days, **or if the registrant contact
   changed** in that window. Worth knowing: a payment-method change is not
   supposed to count, but if Wix also updated the registrant contact as part of
   that change, the clock may have restarted. Wix support can tell you the
   registrant's last-modified date — that's the number that settles it.

If 1–4 are clean and it still fails, it's 5, and no amount of retrying the code
will help. Ask Wix support directly when the domain becomes transferable.

**Once GoDaddy shows the domain in your account**, two bits of tidying: turn
**auto-renew on**, and turn **domain privacy on** (you switched it off in Step 2
so the code could reach you). Auto-renew matters more than it sounds — a lapsed
domain with real traffic gets taken within hours.

Then wait. **Change nothing during the wait.** The Wix page stays up on
`picassodoodles.com`, your real site stays up on `picassodoodlesworldwide.com`,
and nothing is down.

## Step 3 — Point the domain at Netlify

Once GoDaddy shows `picassodoodles.com` in your account:

GoDaddy → **My Products** → `picassodoodles.com` → **Manage DNS**. Add these two
records:

| Type | Name | Value | TTL |
|---|---|---|---|
| **A** | `@` | `75.2.60.5` | 1 hour |
| **CNAME** | `www` | `peppy-licorice-348690.netlify.app` | 1 hour |

`@` means the bare domain. `75.2.60.5` is Netlify's load balancer — it's the same
for every Netlify site, so it looking generic is correct.

Delete any pre-existing A or CNAME record for `@` or `www` that points somewhere
else, or they'll fight.

If you found real MX records at Wix in Step 2, re-create them here too — DNS
doesn't come with the domain, so anything you don't copy across is lost.

**Confirm the CNAME value against your own Netlify site.** I read
`peppy-licorice-348690.netlify.app` off your Firebase screenshot. Netlify
dashboard → your site → the address shown at the top. If it differs, use theirs.

DNS can take up to a day to spread, though it's usually much faster.

## Step 4 — Add the domain in Netlify and make it primary

Netlify → your site → **Domain management** → **Add a domain** →
`picassodoodles.com`.

Then the important bit: **set `picassodoodles.com` as the primary domain.**
Netlify automatically 301-redirects every non-primary domain to the primary one.
That single setting is what makes `picassodoodlesworldwide.com` forward to
`picassodoodles.com` — a permanent 301 is exactly what tells Google to move your
search ranking across, and you get it without configuring anything.

**Keep `picassodoodlesworldwide.com` attached to the Netlify site** as a secondary
domain. That's what performs the redirect. Remove it and old links just break.

Netlify then issues a free SSL certificate automatically, usually within minutes
of DNS resolving. If it doesn't appear, Domain management → **HTTPS** → **Verify
DNS configuration** / **Renew certificate**.

**Test before moving on:** load `https://picassodoodles.com` (should be your
site, with a padlock), then `https://picassodoodlesworldwide.com` (should bounce
to the new domain), then `/admin.html` and request a login code (the text should
arrive — that's Step 1 paying off).

## Step 5 — Now cancel Wix

Only once all three tests above pass:

1. Confirm `picassodoodles.com` no longer appears under Wix → Domains
2. Wix → **Settings** → **Subscriptions** → cancel the Premium plan
3. Delete the Wix site, and close the account if you want

**Download anything you still want off the Wix site first** — photos,
testimonials, copy. Once deleted it's gone.

**Keep `picassodoodlesworldwide.com` registered** at GoDaddy for at least a year.
Every old link, business card, and search result still points there, and the
redirect only works while you own it. It's cheap insurance.

## Step 6 — Tell Google

1. [Search Console](https://search.google.com/search-console) → add
   `picassodoodles.com` as a property
2. Submit `https://picassodoodles.com/sitemap.xml`
3. If `picassodoodlesworldwide.com` is already in Search Console, use the **Change
   of Address** tool to formally move it
4. Update the domain off-site: Instagram bio, Google Business Profile, Good Dog,
   Facebook, TikTok, YouTube

---

## The short version

| # | Do | Then wait for |
|---|---|---|
| 1 | Add the 2 missing hostnames in Firebase | — |
| 2 | Transfer picassodoodles.com, Wix → GoDaddy | registrars, up to 7 days |
| 3 | Add A + CNAME records at GoDaddy | DNS, up to a day |
| 4 | Add domain in Netlify, set it **primary** | certificate, minutes |
| 5 | **Then** cancel Wix | — |
| 6 | Search Console + update social links | — |

The "wait" column is time the internet needs, not time you spend. Your actual
clicking is well under an hour across all six steps. Nothing you buy, and no
hosting to set up.

---

## What changed in the code

- Every `picassodoodlesworldwide.com` in the Terms and Privacy Policy copy is now
  `picassodoodles.com` (3 in `index.html`, 1 in `admin.html`)
- Canonical + Open Graph tags in `index.html` pointing at
  `https://picassodoodles.com/`, so link previews and search settle on the new
  domain rather than splitting between the two
- `noindex` on `admin.html` to keep the admin panel out of search results
- `netlify.toml` — security headers, no-cache on HTML so published edits show up
  immediately, and `X-Robots-Tag` on the admin page
- `robots.txt` and `sitemap.xml`
- **Removed `.htaccess`.** It was written for Apache/cPanel hosting. Netlify
  ignores it completely, so it was dead weight that would have misled whoever read
  this repo next.

`netlify.toml` deliberately has **no `[build]` section**, so your existing publish
settings in the Netlify UI are untouched and adding it can't break the deploy.

The canonical tag names `picassodoodles.com` while the site is still served at the
old domain. That's intentional — it's the correct end state, harmless for a week,
and nothing to remember to change later. If the 60-day lock pushes this out by
months, say so and I'll point it at the old domain in the meantime.

**Deliberately unchanged:**
- Instagram handle `@picassodoodlesworldwide` — a live account, not the domain.
  Changing the link would break it. Rename it on Instagram first if you want them
  to match.
- Contact email `picassodoodlesworldwide@gmail.com` — a Gmail address, unaffected
  by any of this.
- The business name "Picasso Doodles Worldwide" in the policy headers — the
  *domain* shortened; whether the *brand* does is your call.

## Publishing edits

Netlify redeploys automatically when you commit to this repo — edit
`index.html` here and the live site updates on its own within a minute or two.

Day-to-day content (puppies, dogs, inquiries) lives in Firebase, not in these
files. Those edits go through the admin panel and appear instantly.

## One unrelated bug

The "✨ Generate" AI buttons in the admin panel (`aiGen` in `admin.html`) call the
Anthropic API with no API key attached, so they fail every time with "Error. Try
again." Pre-existing and unrelated to this move — flagging so you know the
migration didn't cause it. Fixing it by pasting a key into `admin.html` would
expose that key to anyone viewing page source, so it needs a small server-side
piece instead. Netlify Functions would handle it neatly if you want it working.
