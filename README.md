# PNG → ICO Converter

A lightweight, browser-based tool that converts PNG images to `.ico` files while preserving the **exact original resolution** — no resampling, no quality loss.

## Usage

1. Open `index.html` in any modern browser.
2. Click **Choose File** and select a PNG image.
3. Click **Convert & Download .ico**.
4. The `.ico` file will be downloaded automatically.

## Features

- **100% resolution preserved** — the output ICO has the same pixel dimensions as the input PNG.
- **Full alpha transparency** — uses 32-bit BGRA encoding so transparency is retained.
- **Fully client-side** — nothing is uploaded; all processing happens in your browser.
- **No dependencies** — single HTML file, no libraries, no build step.

## How It Works

1. The PNG is drawn onto an HTML `<canvas>` at its native size using `createImageBitmap`.
2. Raw RGBA pixel data is extracted via `getImageData`.
3. Pixels are converted from RGBA → BGRA and rows are flipped bottom-up (required by the DIB/BMP format inside ICO).
4. A valid `.ico` binary is assembled manually:
   - `ICONDIR` — 6-byte file header
   - `ICONDIRENTRY` — 16-byte image directory entry
   - `BITMAPINFOHEADER` — 40-byte DIB header
   - XOR pixel data (32-bit BGRA)
   - AND mask (zeroed — transparency handled by alpha channel)
5. The result is offered as a download via a `Blob` URL.

## Browser Support

Works in all modern browsers that support:
- `createImageBitmap`
- `CanvasRenderingContext2D.getImageData`
- `Blob` + `URL.createObjectURL`

(Chrome, Firefox, Edge, Safari — all supported.)

## Files

```
index.html   — the converter tool
README.md    — this file
```
