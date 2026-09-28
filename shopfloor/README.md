# MEII Shop Floor

Tablet app for the shop floor: product lists with department sign-off, receiving,
matching received material to product list lines, a job dashboard, and printed
reports. Same setup as the Warehouse Move Tracker: Chrome on the Android tablet,
data stored on the tablet, no server.

## Deploy (GitHub Pages, ~5 minutes)

1. Create a new GitHub repository (private is fine on paid plans; public otherwise).
2. Upload **everything in this folder** (keep the `icons/` and `vendor/` folders).
3. Repository **Settings -> Pages -> Source: Deploy from a branch -> main / root**.
4. Open the Pages URL on the tablet in **Chrome**, then **menu -> Add to Home screen / Install app**.

It must be served over https (GitHub Pages does this). Opening `index.html`
straight from the file system will not work.

After the first load the app works offline, including reading PDFs.

## Updating

Upload the changed files, then in `sw.js` change `VERSION` (for example
`meii-shopfloor-v2`). The tablet picks up the new version on its next two opens.

## Daily use

| Task | Where |
|---|---|
| New product list | **+ Import product list** (top right) -> pick the PDF -> check -> **Create job tab** |
| Another list for the same job (G3, CAB, ENT...) | Import it; it joins the existing job tab |
| Revised list (REV 1) | Job tab -> list -> **Update from PDF**. Sign-offs carry over by part #; lines no longer on the list are kept and marked red |
| Sign a department / packaging / QC | Tap the box, tap the initials |
| Packaged qty | Type it; B/O fills itself with qty - packaged |
| History, box ID, B/O initials + date, notes | Tap the part # |
| Receive material | **Receiving** tab. Job, supplier, PO, slip and bin stay filled for the next item |
| Match received material to a line | Job tab -> **Material** -> **Match to line** (you choose; nothing matches automatically) |
| Move app items | Receiving -> **Import move app file** (JSON or CSV export) |
| Filled PDF on the original form | List -> **Export filled PDF** |
| Put edits made on that PDF back in | List -> **Update from PDF** with the edited PDF |
| Reports | Dashboard -> **Print summary** / **Print full report**; job tab -> **Print job report** |
| Backup | Settings -> **Export backup** (do this daily; keep the file on the network drive) |

A line counts as **done** when all five departments and QC are signed and
nothing is on back order. The dashboard sorts jobs by % of lines done.

## Printer

Receiving labels print to the Zebra ZD621 over Bluetooth LE (same service
UUIDs as the move app). If Bluetooth printing fails, the app offers to print
through the browser print dialog instead. Label size and DPI: Settings.

## Files

| File | What it does |
|---|---|
| `index.html`, `styles.css`, `app.js` | The app |
| `db.js` | On-device storage (IndexedDB). Replace this file to move to a shared backend |
| `parse.js` | Reads product list PDFs from their text layer (exact) |
| `ocr.js` | OCR fallback for scans and photos (from the Product List Converter) |
| `pdfexport.js` | Filled PDF export |
| `zebra.js` | Label ZPL, QR and Bluetooth printing |
| `sw.js`, `manifest.webmanifest` | Offline + install |
| `vendor/` | pdf.js, pdf-lib, tesseract, QR generator |
