# XRAY-ANALYZE — Ceramic Plate Inspection Tool

A single-file, offline browser tool for inspecting X-ray images of ballistic ceramic plates. Built to detect cracks and structural damage using edge detection filters and a magnifier lens — no installation, no dependencies, no internet required.

![screenshot](screenshot.png)

---

## Features

- **TIFF support** — built-in decoder, no external libraries. Supports uncompressed and LZW-compressed TIFF, 8-bit and 16-bit grayscale/RGB
- **Edge detection** — Sobel filter with adjustable threshold to highlight cracks and fractures
- **Four view modes**
  - Original — with brightness, contrast, and gamma corrections
  - Sobel edges — full gradient map
  - Binary threshold — edges above threshold shown in white
  - Overlay — colored edge highlights on top of the original image
- **Magnifier lens** — circular magnifier that follows the cursor, adjustable zoom (2×–12×) and size
- **Image filters** — brightness, contrast, gamma, and invert
- **Overlay colors** — green, red, or cyan edge highlights
- **Zoom & pan** — scroll to zoom, fit-to-screen, keyboard shortcuts
- **Pixel inspector** — live coordinates and RGB values under the cursor

---

## Usage

1. Download `xray_viewer.html`
2. Open it in any modern browser (Chrome, Firefox, Edge)
3. Drag and drop a TIFF (or PNG/JPEG) onto the viewer
4. Use the sidebar controls to adjust filters and switch modes

No server needed. Everything runs locally in the browser.

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `1` | Original view |
| `2` | Sobel edge detection |
| `3` | Binary threshold |
| `4` | Overlay mode |
| `M` | Toggle magnifier |
| `+` / `-` | Zoom in / out |
| `F` | Fit to screen |

---

## TIFF Compatibility

The built-in decoder supports:

| Feature | Supported |
|---------|-----------|
| Uncompressed (type 1) | ✅ |
| LZW compression (type 5) | ✅ |
| 8-bit grayscale | ✅ |
| 16-bit grayscale | ✅ |
| 8-bit RGB | ✅ |
| JPEG-in-TIFF (type 6/7) | ❌ |
| PackBits (type 32773) | ❌ |

If your TIFF uses an unsupported compression type, re-export it as uncompressed from your imaging software (e.g. ImageJ / FIJI: `File → Save As → TIFF`).

---

## Use Case

Developed for non-destructive inspection of ballistic ceramic strike plates after impact testing. X-ray images are loaded and processed to reveal hairline cracks in the ceramic that are difficult to spot with the naked eye.

---

## License

MIT — do whatever you want with it.
