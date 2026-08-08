# Alphabet Soup — Oral Motor Specialists

Website for **Alphabet Soup**, a pediatric speech, feeding, and myofunctional therapy
practice in Norwalk, Connecticut. Clinicians: Bonnie Wilder and Malka Strasberg.

Six static pages. No framework, no build step, **no JavaScript**, no dependencies, and
no external network requests of any kind.

---

## ⚠️ Before this goes live

**The FAQ answers in `faqs.html` are drafts, not verified facts.** They were written as
plausible starting points and several make claims about how the practice actually
operates — insurance, superbills, session length, session frequency. Bonnie and Malka
must read and correct every one of them. The file carries a warning comment above the
list.

Also still outstanding:

| What | Where |
| --- | --- |
| Headshots for Bonnie and Malka | `who-we-are.html` — both show a "Photo coming soon" placeholder |
| Bonnie's credentials, if she wants them listed | `who-we-are.html` — Malka's read "M.S., CCC-SLP, C/NDT"; Bonnie's bio came with none |
| "About our office" blurb | `index.html` — drafted, needs a read-through |
| The four service descriptions | `services.html` — drafted, needs a read-through |

Search the project for `EDIT ME` to find all of them in place.

## Files

```
index.html           Home
services.html        Services — four boxes
what-we-treat.html   Speech / Feeding / Oral Motor + Myofunctional
who-we-are.html      Bios
faqs.html            FAQs  ⚠️ draft answers
contact.html         Phone, email, address
styles.css           All styling; brand palette in the :root block at the top
assets/              Logo, mark, favicons — 216KB total
vercel.json          Clean URLs + security headers
sitemap.xml          All six URLs, for search engines
robots.txt           Allows everything; points at the sitemap
```

## Previewing locally

Clean URLs are on, which means `/services` rather than `/services.html`. **Opening
`index.html` directly over `file://` will break every nav link.** Use a server:

```sh
npx serve .
```

## Editing

**Copy** — all six pages are plain HTML with section comments. Nothing is generated.

**Brand colors** — the `:root` block at the top of `styles.css`. Eight values sampled
from the logo artwork (teal `#0e9cb4`, navy `#0b1f5e`, purple, magenta, orange, green).
Change them there and the whole site follows.

**Adding a headshot** — drop the file in `assets/`, then in `who-we-are.html` replace
the `<div class="photo-placeholder">` block with an `<img>`. The exact markup is in a
comment right above each placeholder. Portrait orientation, ~600px wide, 4:5 frame.

**Contact details** appear on `contact.html` *and* in the footer of all six pages —
update both. Keep `tel:` links in `+1` format (`tel:+19177639785`) even though the
visible text is formatted normally.

## Deploying

No build configuration needed — Vercel serves the files as-is.

```sh
npx vercel          # preview
npx vercel --prod   # production
```

Or import the repo at [vercel.com/new](https://vercel.com/new): Framework Preset
**Other**, and leave build command, output directory, and install command all empty.

### Custom domain

Vercel project → Settings → Domains → add `alphabetsoupspeechtherapy.com`. Vercel
prints the exact records; typically an `A` record on the apex pointing at
`76.76.21.21` and a `CNAME` on `www` pointing at `cname.vercel-dns.com`. Create those
at the registrar. HTTPS is issued automatically once DNS resolves.

If the domain is on Cloudflare, set both records to **DNS only** (grey cloud, not
orange) or certificate issuance will fail.

## How it's built

- **No JavaScript.** FAQ accordions are native `<details>`. The mobile nav has no
  hamburger — below 760px the five links wrap onto a second row under the logo. That
  choice is what keeps the JS at zero; reintroducing a hamburger would end it.
  The one exception, and it is not really an exception: the pages carry
  `<script type="application/ld+json">` blocks. That MIME type tells the browser the
  contents are *data*, not code — it is parsed as JSON by search engines and never
  executed by anything. No behavior, no event handlers, no runtime cost. See
  **Search visibility** below.
- **No external requests.** System fonts only, self-hosted images. Nothing is fetched
  from a CDN, so there is no third party that can see visitor traffic and nothing to
  break if an outside service goes down.
- **Normal scrolling.** No scroll-jacking, no snap points, no scroll-driven animation.
  In-page anchors use native CSS `scroll-behavior`, disabled automatically for visitors
  who have asked for reduced motion.
- **Accessible.** Semantic landmarks, skip link, visible focus rings, `aria-current`
  on the active nav item, and AA contrast throughout.
- **Security headers** in `vercel.json`: HSTS, `X-Frame-Options`, `X-Content-Type-Options`,
  `Referrer-Policy`, `Permissions-Policy`. There is no backend, no database, no form,
  and no user input, so the meaningful risks are account-level — enable 2FA on both the
  Vercel account and the domain registrar, and keep the domain on auto-renew.

## Search visibility

### What's in the repo

| What | Where |
| --- | --- |
| `sitemap.xml` | Root. All six clean URLs, `changefreq` monthly. No `lastmod` — an invented date is worse than none. |
| `robots.txt` | Root. Allows everything, points at the sitemap. Nothing on this site is private. |
| Practice structured data | `index.html` `<head>` — a `MedicalBusiness` / `LocalBusiness` record: name, address, phones, emails, service area, specialties, and both clinicians as `Person` entries. |
| FAQ structured data | `faqs.html` `<head>` — a `FAQPage` block. |
| Breadcrumb structured data | The five non-home pages. |
| Titles and meta descriptions | All six pages — each unique, each naming what the page is about and where the practice is. |
| Service-area copy | Visible paragraphs on the home and contact pages naming Norwalk and the Fairfield County towns. Both are marked `EDIT ME` — confirm the town list. |

**On the "no JavaScript" claim.** The structured data sits in
`<script type="application/ld+json">` tags. That is Google's own recommended format and
it is inert: the browser treats `application/ld+json` as an unknown data type and never
runs it. The site still ships zero lines of executable JavaScript.

**Keeping the FAQ markup honest.** Google requires the `FAQPage` markup to match the
visible answers word for word, and penalises mismatches. The FAQ answers on the page are
still unverified drafts. **When Bonnie and Malka correct an answer, the JSON-LD block at
the top of `faqs.html` has to be corrected to match** — same wording, same punctuation.
There is a comment above the block saying so.

**What was deliberately left out of the structured data**, and should stay out:

- **Opening hours.** The practice does not publish hours. No `openingHours`.
- **Price range.** No real figure to state, so none is stated.
- **Ratings and reviews.** `aggregateRating` and `review` are trivially easy to fake and
  people do it constantly. Inventing them is search spam *and*, in the US, an FTC
  violation. Real reviews belong on the Google Business Profile, where Google collects
  them itself.

### No hidden text, and why

**This site contains no hidden text, no invisible keyword blocks, and no keyword
stuffing — deliberately.** If someone suggests adding 1px text, white-on-white copy, a
`display:none` list of search terms, off-screen keyword paragraphs, or the same phrase
repeated until it stops reading like English: don't. Those techniques were killed off
around 2005. Google detects them mechanically, and the penalty is not a lower ranking —
it is removal from the index, which for a practice that depends on local search is the
whole game. It is also, straightforwardly, deceiving the families the site is meant to
serve.

Every keyword on this site is in a sentence a parent can read. That is the only kind of
optimisation worth having.

### Before launch

On-page work gets a site *eligible* to rank. For a local practice, these off-site items
are what actually decide whether you show up in "speech therapy near me". None of the
work above substitutes for them.

1. **Create and verify a Google Business Profile.** [business.google.com](https://business.google.com).
   For a local practice this outranks everything else on this list combined — it is what
   puts you in the map pack and the sidebar panel. Verification is by postcard or phone
   and takes a couple of weeks, so start it early. Choose a primary category
   (Speech Pathologist), add the address, phone, and photos, and ask happy families to
   leave honest reviews there.
2. **Submit the sitemap in Google Search Console** once `alphabetsoupspeechtherapy.com`
   resolves. [search.google.com/search-console](https://search.google.com/search-console) →
   add the domain property → verify by DNS record → Sitemaps → submit `sitemap.xml`.
   Then use the URL Inspection tool on the home page to request indexing. Search Console
   is also where you will see any structured-data errors.
3. **Get listed in ASHA ProFind** ([asha.org/profind](https://www.asha.org/profind/)) —
   the directory parents are pointed to by pediatricians — plus the Connecticut Speech-
   Language-Hearing Association, and the local Fairfield County parenting and
   practitioner directories. Referral-adjacent listings (dentists, orthodontists,
   pediatricians you already work with linking to the site) are worth more than any
   generic directory.
4. **Keep NAP consistent everywhere.** Name, Address, Phone must be byte-identical across
   the Google Business Profile, ASHA, every directory, and this site:

   > Alphabet Soup — Oral Motor Specialists
   > 83 East Avenue, Suite 313, Norwalk, Connecticut 06851
   > (917) 763-9785

   "Suite 313" vs "Ste 313" vs "#313" reads as three different businesses to Google's
   entity matcher and dilutes all three. Pick the form above and never vary it.
5. **Test the structured data** before and after launch with the
   [Rich Results Test](https://search.google.com/test/rich-results) and the
   [Schema Markup Validator](https://validator.schema.org/).
