# Declaration Desk

Turn scanned supplier documents into a Maldives Customs (ASYCUDA SAD) import worksheet.

Drop a scanned commercial invoice, packing list or B/L (PDF, JPG, PNG, WebP or a phone photo). Claude reads the pages and fills in:

- **Header** – exporter (box 2), consignee and TIN (box 8), invoice no. and date, currency and total invoiced (box 22), delivery terms (box 20), mode of transport (box 25), vessel/flight (box 18), B/L or AWB no., place of loading (box 27), country of export (box 15), total packages (box 6), gross and net mass (boxes 35/38), freight, insurance, other charges and discount.
- **Item lines** – description (box 31), suggested HS code with a confidence rating (box 33), origin (box 34), quantity and unit (box 41), unit price, line total (box 42), packages and package kind, gross and net kg, and Nos (piece count).

Packing-list weights and package counts are merged onto the matching invoice lines.

## Checks

- Qty × unit price vs line total
- Sum of lines vs invoice total, with discount and charges taken into account
- Line weights and package counts vs header totals
- Missing or low-confidence HS codes, missing origin, currency or B/L number

## Export

Set the exchange rate to MVR (box 23). The export spreads freight, insurance and other charges across the lines by value and computes CIF per line in the foreign currency and in MVR.

- **Excel** – two sheets, *Header* and *Items*. The Items sheet can feed `asycuda-sad-generator`.
- **CSV** – item lines only.
- **Copy lines** – tab-separated, for pasting into a spreadsheet.

## Running it

It's a single static page: `index.html`. No build step and no server code.

- **GitHub Pages** – Settings → Pages → Deploy from branch → `main` / root.
- **Locally** – open `index.html` in a browser.

### Reading documents

- **Inside Claude (claude.ai artifact)** – reading uses the viewer's own Claude account.
- **Anywhere else** – open the **Anthropic API key** box and paste a key from [console.anthropic.com](https://console.anthropic.com).
  - The key is kept only in that browser's localStorage.
  - The page calls the Anthropic API directly from the browser, and reads are billed to that key.
  - Don't enter a key on a shared or public computer.
- **Model** – the model field defaults to `claude-sonnet-5`.

Scanned PDFs are rendered page by page with pdf.js. Text-based PDFs also send their text layer, so figures can be cross-checked.

## Caution

HS codes are suggestions. Verify every code, value and weight against the documents and the Maldives Customs tariff before lodging a declaration.
