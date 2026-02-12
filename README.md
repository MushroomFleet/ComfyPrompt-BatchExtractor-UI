# ComfyPrompt BatchExtractor UI

A standalone HTML5 tool for batch-extracting text prompts from ComfyUI-generated PNG images. Upload one or many images, review and adjust which detected string is selected for each, then export matching `.txt` files in one click.

![HTML5](https://img.shields.io/badge/HTML5-Standalone-orange)
![React](https://img.shields.io/badge/React-18-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![No Install](https://img.shields.io/badge/install-none-brightgreen)

---

## How It Works

ComfyUI embeds workflow and prompt metadata inside PNG `tEXt` chunks. This tool parses that binary data directly in the browser, extracts every string value from the embedded JSON, and sorts them by length — the longest string is almost always the positive prompt.

The **offset** control lets you step through the sorted list if the default pick isn't the one you want. Each image can be tuned independently before batch export.

## Features

- **Zero install** — open `demo.html` in any modern browser, no server or build step required
- **Batch upload** — drag-and-drop or browse for multiple PNGs at once
- **IndexedDB persistence** — your queue survives page reloads and browser restarts
- **Offset selector** — cycle through all detected strings per image to pick the correct prompt
- **Expandable preview** — click any prompt preview to see the full text
- **Batch export** — one-click download of `.txt` files, each named to match its source image
- **Error handling** — clear badges and toasts for non-PNG files, stripped metadata, or empty results
- **Thumbnails** — compressed JPEG thumbnails stored in IndexedDB so the list stays visual without eating memory

## Getting Started

1. **Clone the repository**

```bash
git clone https://github.com/MushroomFleet/ComfyPrompt-BatchExtractor-UI.git
cd ComfyPrompt-BatchExtractor-UI
```

2. **Open the app**

Open `demo.html` in your browser. That's it — no `npm install`, no build, no server.

> On most systems you can just double-click the file, or run `open demo.html` / `xdg-open demo.html` from the terminal.

3. **Upload images**

Drop ComfyUI PNG files onto the drop zone (or click to browse). The tool accepts multiple files at once.

4. **Review prompts**

Each image appears as a card showing:
- A thumbnail preview
- The output filename (`imagename.txt`)
- The currently selected prompt text
- An offset control showing how many strings were found

Use the **−** and **+** buttons to change the offset if the default selection isn't the prompt you want. Click the preview text to expand it fully.

5. **Export**

Hit **Export All as .txt** in the toolbar. Each valid entry downloads as a plain text file with the same base name as the source PNG.

## File Structure

```
ComfyPrompt-BatchExtractor-UI/
├── demo.html                              # Complete standalone application
├── comfyprompt-extractor.jsx              # Reusable React hook (for integration into other projects)
├── ComfyUI-Prompt-Extractor-JSX-integration.md  # Integration guide for the JSX hook
└── README.md
```

## How Offset Selection Works

When a PNG is processed, the tool:

1. Parses all `tEXt` and `iTXt` chunks from the PNG binary
2. Extracts every string value found inside any embedded JSON
3. Deduplicates and sorts them by length, longest first

| Offset | Typical Content |
|--------|----------------|
| 0 | Positive prompt (longest string) |
| 1 | Negative prompt (usually second longest) |
| 2+ | Sampler names, model names, LoRA tags, short parameters |

If your workflow produces unusually long negative prompts or other metadata, just bump the offset to find the string you actually want.

## Browser Compatibility

Tested in:
- Chrome 80+
- Firefox 75+
- Safari 13.1+
- Edge 80+

Requires `File.arrayBuffer()`, `DataView`, `TextDecoder`, and `IndexedDB` — all widely supported in modern browsers.

## Troubleshooting

**No prompt extracted from my PNG**
The image may have had its metadata stripped. Social media platforms, screenshot tools, and some image editors remove PNG text chunks when saving. Use the original file from ComfyUI's output folder.

**Browser blocks multiple downloads**
Some browsers throttle rapid sequential downloads. The tool adds a 300ms delay between files, but if your browser still blocks them, try allowing popups/downloads for the page, or export in smaller batches.

**Entries disappear after clearing browser data**
The queue is stored in IndexedDB. Clearing site data or using incognito mode will reset it.

## 📚 Citation

### Academic Citation

If you use this codebase in your research or project, please cite:

```bibtex
@software{comfyprompt_batchextractor_ui,
  title = {ComfyPrompt BatchExtractor UI: Batch prompt extraction tool for ComfyUI PNG images},
  author = {[Drift Johnson]},
  year = {2025},
  url = {https://github.com/MushroomFleet/ComfyPrompt-BatchExtractor-UI},
  version = {1.0.0}
}
```

### Donate:

[![Ko-Fi](https://cdn.ko-fi.com/cdn/kofi3.png?v=3)](https://ko-fi.com/driftjohnson)
