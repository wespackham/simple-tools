# simple-tools

Simple, single-purpose web tools. Live at **https://tools.abrupt.app**.

## Tools

- **Straighten Punctuation** — paste text from any word processor (Google Docs, Word, etc.) and curly punctuation (`’ “ ” — … –`) is converted to straight equivalents. Fonts, sizes, colors, hyperlinks, and all other formatting are preserved exactly. Copy the result out as rich text and paste it into another document.

## How it works

One static page (`index.html`), no build step, no dependencies, no tracking. Paste rich text, punctuation is straightened in-place via a regex table (`CURLY_MAP`), and the result is copied back to the clipboard as rich text.

## Deployment

GitHub Pages serves the `main` branch at `tools.abrupt.app` (custom domain via `CNAME`, DNS hosted on Cloudflare — DNS only, not proxied). Pushing to `main` triggers a rebuild automatically.

## Local use

No build step — open `index.html` in a browser, or serve the folder:

```
python3 -m http.server
```

Clipboard write (Copy) requires a secure context, so use `http://localhost` or the deployed site rather than `file://`.
