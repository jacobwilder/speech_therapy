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
