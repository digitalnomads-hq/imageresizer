# Image Resizer

**Built by [Digital Nomads HQ](https://dnhq.com.au)**

A fast, client-side image resizing and compression tool. Drag in a batch of images, pick your sizes, and download — nothing ever leaves your browser.

![Single HTML file](https://img.shields.io/badge/single%20file-HTML-blue) ![No server required](https://img.shields.io/badge/server-not%20required-green) ![Privacy first](https://img.shields.io/badge/privacy-100%25%20local-purple)

---

## Features

- **Drag & drop** or click to select — supports JPG, PNG, WebP, GIF, AVIF
- **Multi-size processing** — select one or more size presets and process all images at every size in one click
- **MozJPEG + OxiPNG compression** — uses WASM-based professional codecs (the same ones used by Google's Squoosh) for significantly better compression than the browser's built-in encoder
- **Before/after compare slider** — drag to compare original vs compressed quality on any image
- **Flexible filename suffixes** — append the size automatically (e.g. `photo-2560w.jpg`), a custom suffix, or nothing
- **Multiple output formats** — output as JPEG, PNG, WebP, or keep the original format
- **Per-image or bulk download** — download individual images or all processed images as a ZIP
- **Stats bar** — shows total original size, processed size, and how much was saved
- **No uploads, no server, no tracking** — everything runs in your browser

---

## Size Presets

| Preset | Max Width | Quality | Use case |
|---|---|---|---|
| Web 2x / Retina | 2560px | 85% | High-DPI displays, full-width heroes |
| Web Hero | 1920px | 85% | Standard full-width images |
| Web Large | 1280px | 82% | Wide content images |
| Web Standard | 800px | 80% | Body content, blog images |
| Thumbnail | 400px | 75% | Cards, previews, grid items |
| Social Media | 1080px | 88% | Instagram, Facebook, etc. |
| Compress Only | Original | 92% | Reduce file size without resizing |

You can also add custom widths on the fly.

---

## Compression Quality

The quality slider (1–100) applies to all processing. JPEG and WebP use it directly as a lossy quality parameter. PNG is always lossless — the slider has no effect on PNG output, but OxiPNG still significantly reduces file size compared to the browser's default encoder.

### Codec stack

| Format | Encoder | Notes |
|---|---|---|
| JPEG | **MozJPEG** (WASM) | Mozilla's optimised JPEG encoder — typically 20–35% smaller than standard libjpeg at the same visual quality |
| PNG | **OxiPNG** (WASM) | Lossless PNG optimiser — rewrites deflate stream for smaller output |
| WebP | **libwebp** (WASM) | Google's reference WebP encoder |
| Fallback | Browser canvas | Used automatically if WASM fails to load |

Codecs load in the background from [esm.sh](https://esm.sh) on first visit and are cached by the browser. If they fail to load (e.g. offline), the tool falls back to the browser's built-in encoder gracefully.

---

## Usage

### Self-hosted / WordPress

Just drop `index.html` into any directory on your server and open it in a browser. No build step, no dependencies to install, no server-side code.

```
yourdomain.com/tools/imageresizer/index.html
```

### Local

Open `index.html` directly in any modern browser. Everything works offline except the WASM codec loading (which requires an internet connection on first load).

---

## How It Works

1. Images are loaded into an HTML5 `<canvas>` element for resizing
2. The resized pixel data is passed to the appropriate WASM encoder (MozJPEG / OxiPNG / libwebp)
3. The encoded output is stored as a Blob in memory — never written to disk, never uploaded
4. Downloads are triggered via a temporary object URL

ZIP files are built in-browser using [fflate](https://github.com/101arrowz/fflate). Images are stored without re-compression inside the ZIP since they're already in compressed formats.

---

## Browser Support

Works in all modern browsers (Chrome, Firefox, Safari, Edge). Requires support for:
- Canvas API
- WebAssembly
- CompressionStream (for ZIP — falls back gracefully if unavailable)
- ES2020+

---

## External Dependencies

Both are loaded from CDN and cached after first load:

| Library | Source | Purpose |
|---|---|---|
| `@jsquash/jpeg`, `@jsquash/png`, `@jsquash/webp` | esm.sh | WASM image encoders |
| `fflate` | jsDelivr | In-browser ZIP creation |

If you need the tool to work fully offline with no external requests, the dependencies can be downloaded and bundled into the HTML file.

---

## Development

There's no build system. Edit `index.html` directly.

```bash
git clone https://github.com/digitalnomads-hq/imageresizer.git
cd imageresizer
open index.html
```
