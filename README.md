# ljcontentlibrary

A lightweight, self-hosted video library tool for LexJet sales teams. Displays the latest product videos from YouTube across all subsidiary channels, with real-time search, filtering by brand/model/technology, duration display, and one-click link copying.

Designed to be embedded in SharePoint pages via GitHub Pages hosting.

---

## How it works

Video metadata is stored in a JSON file generated from a CSV spreadsheet. The HTML tool fetches that JSON and renders a filterable card grid — no backend, no database, no build step required.

---

## File structure

```
content-library/
├── lj/
│   ├── index.html          # LexJet tool
│   ├── videos.csv          # Edit this to add/update LexJet videos
│   └── videos.json         # Generated — do not edit manually
├── bmg/
│   ├── index.html          # BMG tool
│   ├── videos.csv
│   └── videos.json
├── dgp/
│   ├── index.html          # DIGIPRINT tool
│   ├── videos.csv
│   └── videos.json
├── solp/
│   ├── index.html          # SOLP tool
│   ├── videos.csv
│   └── videos.json
├── csv_to_json.py          # Shared converter script
└── README.md
```

---

## Adding or updating videos

1. Open the subsidiary's `videos.csv` in Excel (e.g. `lj/videos.csv`)
2. Add or edit rows — one row per video
3. Run the converter from Terminal:
   ```bash
   cd "/Users/nate.dodson/Documents/Claude/Code Projects/video-library"
   python3 csv_to_json.py lj/videos.csv lj/videos.json
   
   ```
4. Commit and push both `videos_template.csv` and `videos.json` to GitHub
5. The live page updates automatically within a minute

---

## CSV column reference

| Column | Description | Example |
|---|---|---|
| `id` | YouTube video ID (the part after `?v=` in the URL) | `dQw4w9WgXcQ` |
| `title` | Video title | `HP DesignJet T650 Setup Guide` |
| `description` | Full video description | `Complete unboxing and setup...` |
| `published` | Upload date | `2024-11-15` or `11/15/2024` |
| `brand` | Printer brand | `HP` |
| `model` | Printer model | `DesignJet T650` |
| `technology` | Print technology | `Thermal Inkjet` |
| `format` | `landscape` for standard videos, `short` for YouTube Shorts | `landscape` |
| `duration` | Video duration | `12:34` |

---

## Deploying a new subsidiary version

1. Copy `index.html` and rename it (e.g. `subsidiary-name.html`)
2. Edit the `CONFIG` block at the top of the file:
   ```js
   title:    "Subsidiary Name — Video Library",
   jsonUrl:  "https://yourorg.github.io/ljcontentlibrary/subsidiary-videos.json",
   accent:      "#1a1a2e",   // header color
   accentLight: "#3a86ff",   // highlight color
   ```
3. Generate a separate CSV and JSON for that subsidiary
4. Embed the page in SharePoint:
   ```html
   <iframe src="https://yourorg.github.io/ljcontentlibrary/subsidiary-name.html" width="100%" height="900" frameborder="0"></iframe>
   ```

---

## Local testing

To preview the tool without deploying, set `INLINE_DATA` in the CONFIG block to the contents of your `videos.json` file. Set it back to `null` before pushing to GitHub.

---

## Requirements

- Python 3 (for running the CSV converter)
- A GitHub account with GitHub Pages enabled on this repository
- No other dependencies

---

© 2026 S-One Holdings. All rights reserved. See [LICENSE](LICENSE) for terms.
