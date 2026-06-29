# HD Passport

A digital "passport" for How College's Digital Programmes. Visitors collect a
stamp from each stand by scanning a QR code; once all six stamps are collected,
they're invited to enter a prize raffle.

It's a static site — plain HTML, CSS and JavaScript with no build step — hosted
on GitHub Pages.

**Live site:** https://sobelements.github.io/hdpassport/

## How it works

1. A visitor opens the site on their phone. The landing page redirects to the
   **passport**, which shows six empty stamp slots.
2. At each stand there's a printed **QR code**. Scanning it opens that stand's
   **stamp page**, which records the stamp in the browser's `localStorage`, shows
   a brief "Stamping…" animation, then returns to the passport — where the stamp
   now appears as collected.
3. When all six stamps are collected, a **celebration popup** appears. The
   visitor enters their name and taps a button, which opens their email app with
   a pre-filled raffle entry addressed to `howdigital@howcollege.ac.uk`.

Progress is stored per-device in `localStorage` under the key
`howdigital_passport`. There's no server or database — clearing the browser's
storage resets the passport.

## The six stamps

| ID (folder / storage key) | Label              |
| ------------------------- | ------------------ |
| `how-digital`             | How Digital        |
| `blc`                     | BLC                |
| `digital-programmes`      | Digital Programmes |
| `he`                      | HE                 |
| `aqp`                     | AQP                |
| `construction`            | Construction       |

## Project structure

```
index.html              Landing page — redirects to passport/
passport/index.html     The passport: stamp grid + raffle celebration
stamp/<id>/index.html   One per stamp — records the stamp, then redirects back
qr/index.html           Printable page that generates a QR code per stand
css/style.css           All styling
js/qrcode.min.js        QR code library (used only by qr/)
assets/
  passport-bg.png       Passport background artwork
  stamps/               Stamp images shown on the passport
  stamps/_original/     Unoptimised source images (kept for re-editing)
.nojekyll               Tells GitHub Pages to serve files as-is
```

## Printing the QR codes

Open [`qr/index.html`](qr/index.html) (locally or at
`/qr/` on the live site) and click **Print**. It generates one QR code per
stand, each linking to that stand's stamp page. This page is the single source
of truth for the printed codes — the base URL is set at the top of the file's
script (`var BASE = '…'`).

## Local development

No build or server is strictly required. To preview, either open the HTML files
directly in a browser, or serve the folder so paths resolve cleanly:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000/
```

## Deployment

The site is published via **GitHub Pages** from the
[`SobElements/hdpassport`](https://github.com/SobElements/hdpassport) repository.
Pushing to the deployment branch updates the live site automatically. The
`.nojekyll` file ensures all files are served without Jekyll processing.

> **Note:** the QR base URL in [`qr/index.html`](qr/index.html) is hard-coded to
> the GitHub Pages address above. If the site ever moves to a different host or
> account, update that value and re-print the codes.

## Adding or changing a stamp

1. Add the stamp image to `assets/stamps/`.
2. Create `stamp/<id>/index.html` (copy an existing one and change `STAMP_ID`
   and the displayed label).
3. Add the stamp's slot and image to `passport/index.html`, and add its `id` to
   the `STAMPS` array in that file's script.
4. Add the stamp to the `STAMPS` array in `qr/index.html` so a code is printed.
5. Re-print the QR codes.

## Limitations

The raffle is honour-system: stamps live in the visitor's browser only, so they
can be cleared or manually set, and entries arrive as plain emails. That's fine
for a fun event, but it isn't fraud-proof.
