# XRAY-ANALYZE — Ceramic Plate Inspection Tool

A single-file, offline browser tool for inspecting X-ray images of ballistic ceramic plates. Built to detect cracks and structural damage using edge detection, automated filter optimization, and crack candidate analysis — no installation, no dependencies, no internet required.


<img width="1900" height="846" alt="image" src="https://github.com/user-attachments/assets/d72c7575-d2ab-4291-b325-9f9411e22707" />

---

## Features

- **TIFF support** — built-in decoder, no external libraries. Supports uncompressed and LZW-compressed TIFF, 8-bit and 16-bit grayscale/RGB
- **Edge detection** — Sobel filter with adjustable threshold to highlight cracks and fractures
- **Four view modes**
  - Original — with brightness, contrast, and gamma corrections
  - Sobel edges — full gradient map
  - Binary threshold — edges above threshold shown in white
  - Overlay — colored edge highlights on top of the original image
- **Auto-optimize** — one-click automatic filter calibration per image (see below)
- **Crack candidate detection** — connected components analysis with bounding boxes and severity scoring (see below)
- **Magnifier lens** — circular magnifier that follows the cursor, adjustable zoom (2×–12×) and size
- **Image filters** — brightness, contrast, gamma, and invert
- **Overlay colors** — green, red, or cyan edge highlights
- **Zoom & pan** — scroll to zoom, fit-to-screen, keyboard shortcuts
- **Pixel inspector** — live coordinates and RGB values under the cursor
- **Export PNG** — downloads the current view including any crack bounding boxes

---

## Usage

1. Download `xray_viewer.html`
2. Open it in any modern browser (Chrome, Firefox, Edge)
3. Drag and drop a TIFF (or PNG/JPEG) onto the viewer
4. Click **Auto** to automatically calibrate the filters for the loaded image
5. Switch to **Overlay** mode and run **Analyseer scheuren** to detect crack candidates
6. Click any crack in the results list to zoom in on it

No server needed. Everything runs locally in the browser.

---

## Auto-Optimize

The **Auto** button (next to Reset in the image filters section) analyzes the pixel histogram of the loaded image and automatically sets the best brightness, contrast, gamma, and edge detection threshold — in one click.

The algorithm uses three techniques:

**Percentile clipping for brightness & contrast** — the histogram is analyzed to find the 1st and 99th percentile pixel values. The contrast is stretched so that this range fills the full 0–255 output range, cutting off noise at both ends and maximizing the visible dynamic range of the scan.

**Adaptive gamma correction** — the mean brightness of the image is used to nudge the gamma up (for dark images, to lift midtones) or down (for overexposed images). This ensures structural detail in the ceramic is visible regardless of original exposure settings.

**Otsu's threshold** — Otsu's method finds the statistically optimal threshold that maximizes the variance between the background and edge classes. This value is then scaled to the edge map range to give a sensible starting point for crack detection without manual tuning.

After applying, a small info panel shows exactly what was detected: histogram range, mean pixel value, and all applied values. Clicking **Reset** restores defaults and hides the panel.

---

## Crack Candidate Detection

The **Scheurdetectie** section runs a fully local, offline crack detection pass on the current edge map. No data ever leaves the browser.

### How it works

1. The current thresholded edge image is binarized into a pixel mask
2. A **BFS connected components** algorithm (8-connected neighbors) labels every distinct group of edge pixels
3. Each component's **bounding box** is measured for length and aspect ratio
4. Components that are long enough and sufficiently elongated (length >> width, like a crack) are kept as candidates
5. Candidates are sorted by length and drawn on an overlay canvas with **color-coded bounding boxes**

### Severity colors

| Color | Meaning |
|-------|---------|
| 🟡 Yellow | Short crack candidate (< 80 px) |
| 🟠 Orange | Medium crack candidate (80–200 px) |
| 🔴 Red | Long crack candidate (> 200 px) |

### Parameters

| Parameter | What it controls |
|-----------|-----------------|
| Min. lengte | Minimum bounding box length in pixels to be considered a crack |
| Min. aspect ratio | How elongated the bounding box must be (e.g. 3.0 = at least 3× longer than wide) |
| Doos dikte | Padding around each bounding box in pixels |

### Clicking results

Clicking any entry in the results list zooms the viewer into that crack candidate and highlights its bounding box with a solid border. The exported PNG includes all bounding boxes.

> **Tip:** Run **Auto** first to calibrate the edge threshold, then run crack detection. Adjust the threshold slider if too many false positives appear (increase it) or real cracks are missed (decrease it).

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

### Multi-IFD TIFFs — automatic highest resolution selection

Many X-ray and scanner applications write TIFF files that contain multiple IFDs (Image File Directories) — essentially multiple images packed into a single file. A common pattern is a small RGB thumbnail in the first IFD followed by the full-resolution 16-bit grayscale scan in a later IFD.

The viewer automatically walks all IFDs in the file and loads the one with the highest pixel count. This means you always get the full-resolution image, regardless of where it sits in the file. The info panel shows how many layers were found and confirms that the best one was selected.

Example from a real scan file:

| IFD | Resolution | Bit depth | Role |
|-----|-----------|-----------|------|
| #0 | 256 × 205 px | 8-bit RGB | Embedded thumbnail (skipped) |
| #1 | 3072 × 3072 px | 16-bit grayscale | Full scan ← loaded |

---

## Use Case

Developed for non-destructive inspection of ballistic ceramic strike plates after impact testing. X-ray images are loaded and processed to reveal hairline cracks in the ceramic that are difficult to spot with the naked eye.

---

## License

MIT — do whatever you want with it.
