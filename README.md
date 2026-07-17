# news-reader

Daily RSS → EPUB digest, built automatically by GitHub Actions.

## How it works

Every day at **6:00 AM CT** (11:00 UTC) the [build workflow](.github/workflows/build-daily-epub.yml)
reads [`feeds.json`](feeds.json), collects everything published in the last 24 hours,
fetches full article text where a feed only carries an excerpt (verbatim — no
summarizing), and commits:

- **`daily.epub`** — always the latest edition
- **`archive/MM-DD-YYYY.epub`** — dated copy
- **`build-report.md`** — per-feed status of the last run
- **`state/seen.json`** — GUIDs of already-published articles (prevents repeats)

Each epub includes a cover image with the date and article count for e-reader
thumbnail display.

## Maintaining the feed list

Edit `feeds.json` and push — the next build picks it up automatically.

```jsonc
{
  "settings": {
    "title": "Daily News",
    "timezone": "America/Chicago",
    "windowHours": 24
  },
  "categories": [
    {
      "name": "Tech",
      "feeds": [
        {
          "name": "Ars Technica",
          "url": "https://feeds.arstechnica.com/arstechnica/index",
          "fullText": true
        }
      ]
    }
  ]
}
```

## Running manually

Trigger the **Build daily epub** workflow from the Actions tab
(`workflow_dispatch`), or locally:

```sh
pip install -r requirements.txt
python build_epub.py
```
