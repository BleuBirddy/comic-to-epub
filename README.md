# Comic to EPUB

Turns CBZ, CBR and PDF comics into fixed-layout EPUBs, with foggy speech-bubble
lettering cleaned up. Everything runs in the visitor's browser: no server, no uploads.

Use it here: <https://bleubirddy.github.io/tools/comic-to-epub/>

## Run it yourself

No build step and nothing to install. All libraries are inside `lib/`. The
page loads workers and WebAssembly, so it has to be served rather than opened
as a file:

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000/>. To host it, copy the folder to any static
host (GitHub Pages works).

The "Bleubirddy tools" link and the tab icon point at `/tools.html` and
`/favicon.svg` on bleubirddy.github.io. Change or remove them in `index.html`
if you host it somewhere else.

## Files

| File | What it does |
|---|---|
| `index.html`, `style.css`, `ui.js` | The page: drop zone, options, progress, downloads |
| `app.js` | Reads CBZ/CBR/PDF, runs pages through the workers, writes the EPUB |
| `page-worker.js` | Splits two-page spreads and cleans up lettering (off the main thread) |
| `comic_convert.py` | The command-line version (Python). It can also re-letter bubbles with OCR; its options are described at the top of the file |
| `lib/jszip/` | JSZip 3.10.1: reads CBZ, writes EPUB (MIT) |
| `lib/libarchive/` | libarchive.js 2.0.2: reads CBR/RAR via WebAssembly (MIT) |
| `lib/pdfjs/` | pdf.js 4.10.38: renders PDF pages (Apache-2.0) |

## Restyling

The colors are CSS variables at the top of `style.css` (light and dark sets). To match
the rest of the site, change those, or replace the `<header>`/`<footer>` in `index.html`
with your site's own.

## Notes

- **Speed:** about 15-35 seconds per issue on a laptop; slower on a phone.
- **Output size:** close to the original. "Maximum image quality" roughly doubles it.
- **Browsers:** current Chrome, Edge, Firefox and Safari (needs OffscreenCanvas and
  module workers: Safari 16.4 or newer).
- **Not in this version:** OCR re-lettering (`comic_convert.py`'s `--lettering reletter`).
  Planned as version 2.

## License

MIT, see [LICENSE](LICENSE). The libraries in `lib/` keep their own licenses
(see each folder).
