# Batch Reconciliation

A single self-contained HTML file that reconciles telecom site-upgrade paperwork across a
whole batch: **PO → WO → BOQ → GCL (handover) → PAC (acceptance) → POD (dismantled-asset
return)**.

No backend, no build step, no install. Open `index.html` in a browser and it works.

## Privacy

Everything happens in your browser. Documents are parsed in memory, held for the session,
and gone when you refresh — nothing is uploaded, transmitted or stored. The only thing
written to browser storage is an optional client ID if you connect OneDrive.

This file contains no real project data.

## What it does

Upload a batch as a single workbook, several files, a whole folder, or a `.zip` — mixed
PDFs and spreadsheets are fine. The tool classifies each document itself and matches sheets
and columns by their content rather than by name, so renamed tabs and reordered columns
don't break it.

It then checks, per site:

- **Document identity** — do the Site ID and WO number inside each document actually match
  the work order?
- **BOQ** — item codes and quantities against the GCL as-built, and amounts against the WO
  where the sheet carries prices
- **Quantities** — as-built vs design
- **Signatures** — contractor and MSP sign-off presence
- **Dates** — acceptance, sign-off and delivery in a plausible order
- **Dismantled assets** — what the GCL says was removed vs what the POD returned, matched
  on asset ID
- **Asset tags** — duplicates, and tags appearing as both newly installed and returned

Findings are graded: **yellow** for anything needing review (dates, signatures, quantity
variance), **red** only for a document that is missing or rejected. Findings shared across
many sites are rolled up so a batch-wide pattern reads as one issue, not fifty.

Results export to CSV, a multi-sheet Excel workbook, or a standalone HTML report.

## Two profiles

- **CAPEX9** — tower/generator replacement: PO → WO → BOQ → GCL → PAC → POD
- **5G Battery / DC** — DAR → BOQ → GCL → Warranty → POD, matched on Site ID, with support
  for an approved "invoice without POD" exception

## Development

Validated against 91 real batch folders from 10+ contractors (~1,700 documents), with four
verification suites covering PDF and workbook extraction, the reconciliation rules, the
interface, and every ingestion path.

## Licence

MIT
