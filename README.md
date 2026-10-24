# Caben

**A complete kitchen cabinet refacing website — one HTML file, no build step.**

### [Open the live site →](https://sf-ryanyan.github.io/caben/)

A homeowner lands on the page, drags a before/after slider, counts their doors,
picks a finish, and sees a real price range — then books a free measurement.
The business owner sees the request come in, and can change the company name,
colours and prices without touching code.

Caben is a fictional brand. The site is a working demonstration of what a small
home-services company can put online.

---

## What a visitor can do

| On the page | What happens |
|---|---|
| **Drag the before/after slider** | The same kitchen, old doors and new, under one handle. Works with the mouse, a finger, or the arrow keys. |
| **Count their kitchen** | Upper doors, lower doors, drawer fronts. Plus and minus buttons, no form to fill in. |
| **Pick a door style and finish** | Four door styles, six finishes. The photo above swaps to match the colour they choose. |
| **See the price build up** | A range appears and updates as they change anything, with every line item shown underneath. |
| **Read the price list** | The actual per-door and labour rates are on screen, not hidden behind a "contact us". |
| **Browse recent work** | Four projects with door count, finish and time on site. |
| **Book a visit** | Name, mobile, city, address, preferred time. They get a summary they can copy. |
| **Switch language or theme** | English and Chinese, light and dark. The choice is remembered. |

---

## What the business owner gets

Three things on this page are aimed at the owner rather than the customer.

**A live rebrand panel.** The "Make it yours" button opens a panel where the
company name and the brand colour change across the whole page as you type.
It is there so an owner can see their own name on the site inside ten seconds,
before any work is quoted.

**An owner view.** Near the bottom, a mock dashboard shows what an incoming
request looks like: who, where, how many fronts, the estimated range, and when
it arrived. It answers the first question every owner asks — *where do the
leads go?*

**A visible price list.** The rates are printed on the estimate screen and are
editable in the code. An owner can hand the page to a customer and walk through
how the number was reached.

---

## How the estimate is calculated

Nothing is random. The whole figure comes from this:

```
base      = upper doors   x upper price
          + lower doors   x lower price
          + drawer fronts x drawer price

hardware  = total fronts  x handle price        (0 if "Keep mine")

labour    = tier by total fronts:  up to 15 -> l1
                                   16 to 25 -> l2
                                   26 and up -> l3

total     = base + hardware + labour + optional faucet + optional sink

range     = total, plus and minus a set percentage, rounded to the nearest $50
```

Default rates in this demo: $85 per upper door, $95 per lower door, $65 per
drawer front, $14 per handle; labour $450 / $700 / $950 by tier; faucet $380,
sink $540; range plus or minus 10 percent.

---

## Make it your own

Everything a business would change sits in one `CONFIG` block near the top of
the script, written in plain English:

```js
brand:  "CABEN",
area:   "San Francisco Bay Area",
cities: ["San Jose", "Santa Clara", "Sunnyvale", ...],

prices: {
  upper: 85, lower: 95, drawer: 65, handle: 14,
  l1: 450, l2: 700, l3: 950,
  faucet: 380, sink: 540, range: 10
},

styles:   [ ... four door styles, each with its photo ... ],
finishes: [ ... six colours, each with a hex value and a photo ... ],
handles:  [ ... five hardware options ... ],
palette:  [ ... five brand colours for the rebrand panel ... ]
```

Change a number and the whole page follows — the estimate, the breakdown and
the printed price list all read from the same values.

All wording lives in an `L` object with an English and a Chinese version of
every string, so the site can be translated without hunting through the markup.

> **Never put an API key or any other secret in this file.** It is public —
> anyone can read the source, and leaked keys get found and abused within
> minutes. Keys belong on a server (a Cloudflare Worker or a Vercel function),
> never in a static page.

---

## Receiving leads

By default there is no server. A submitted request is shown back to the
customer with a **Copy these details** button, alongside the company's name and
number. Nothing leaves the browser.

To receive leads by email instead, create a free form at
[Formspree](https://formspree.io) and paste its URL into `CONFIG.formEndpoint`:

```js
formEndpoint: "https://formspree.io/f/xxxxxxx"
```

Any endpoint that accepts a JSON `POST` works the same way.

---

## Built with

One `index.html` file of about 100 KB, plus an `images` folder. No framework,
no package manager, no build step, no backend. Open the file in a browser and
it runs.

Two things are loaded from a CDN — Google Fonts for the typefaces, and a small
library that draws the QR code in the footer. Both fail gracefully: the page
falls back to system fonts and hides the QR block rather than breaking.

Also in the box:

- **Responsive** — designed separately for desktop, tablet and phone, not
  shrunk down from one layout
- **Light and dark** — both designed, not inverted; follows the system setting
  and can be overridden
- **Bilingual** — English and Chinese throughout, including the estimate
- **Accessible** — keyboard operable, visible focus states, text contrast
  checked, and it respects `prefers-reduced-motion`
- **Works without JavaScript** — the page still reads top to bottom; only the
  interactive parts stop

---

## Publish your own copy

1. Create a repository and upload `index.html` and the `images` folder to the root.
2. **Settings → Pages → Source: Deploy from a branch → `main` / `(root)`.**
3. The site appears at `https://YOUR-USERNAME.github.io/REPO-NAME/` within a
   minute or two.

No other setup is required.

---

## What it does not do

Worth saying plainly, because these are the three things people ask for next:

- **No photo recognition.** The customer can attach a photo, but it is a local
  preview only — it never leaves the browser. Counting doors automatically
  needs an AI vision model running on a server with its own API key.
- **No payments or scheduling.** The booking form collects a request; it does
  not take a deposit or hold a slot in a calendar.
- **No real admin backend.** The owner view is a demonstration of the idea, not
  a working CRM. Leads arrive by email through Formspree, or on the customer's
  own screen to read out over the phone.

All three belong in a server-backed version. This page covers the part that
gets a stranger from "I wonder what that costs" to a booked appointment.

---

## Disclaimer

Caben is a fictional brand created for this demonstration. The company, the
projects, the reviews and the prices shown are illustrative and do not describe
a real business. The service area names real Bay Area cities only to make the
example concrete.

---

## License

MIT. Use it commercially, rebrand it, sell work built on it. No attribution
required.

---

## About

Built by **Ryan Yan**, a data analyst in the San Francisco Bay Area, as a
demonstration of what a small local business can put online — designed, written
and coded end to end.

[github.com/sf-ryanyan](https://github.com/sf-ryanyan) ·
[Live site](https://sf-ryanyan.github.io/caben/)
