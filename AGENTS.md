# AGENTS.md — simple-tools

Simple, single-purpose web tools. Deployed via GitHub Pages at **https://tools.abrupt.app**.

## Project

- **Repo:** https://github.com/wespackham/simple-tools (branch `main`, HTTPS remote — SSH keys are not set up for GitHub on this machine; use `gh auth setup-git` if it breaks)
- **Hosting:** GitHub Pages, deploy-from-branch (`main` / root), custom domain `tools.abrupt.app` via the `CNAME` file
- **DNS:** `tools.abrupt.app` is a Cloudflare DNS record (CNAME → `wespackham.github.io`, **DNS only / grey cloud** — must not be proxied, or GitHub cannot issue its TLS cert)
- **Stack:** plain HTML/CSS/JS in a single `index.html`. No build step, no dependencies, no framework.

## Layout

- `index.html` — the whole site: styles, markup, and JS in one file
- `CNAME` — custom domain for GitHub Pages (managed by Pages settings; GitHub may commit/delete it automatically when domain settings change — don't delete it)

## Conventions

- Each tool is a self-contained block in `index.html` (currently one: Straighten Punctuation). Keep it that way unless the site grows.
- The punctuation conversion table lives in `CURLY_MAP` in the inline script. Add new mappings there (regex → replacement, applied in order).
- Formatting must be preserved through any transformation: text is straightened only in text nodes, never in URLs/attributes, and pasted HTML is not otherwise rewritten.
- No external assets, trackers, or build tooling. Vanilla DOM APIs only.

## Workflow

- Commit messages: short imperative summaries (existing history is a good reference).
- After pushing to `main`, GitHub Pages rebuilds automatically (check: `gh api repos/wespackham/simple-tools/pages/builds/latest --jq .status`). Hard-refresh (Cmd+Shift+R) when verifying — browsers cache aggressively.
- Never commit secrets; there are none in this repo and none should ever be needed.

## Testing

No formal test suite. To verify changes, the established ad-hoc approach:

1. **DOM logic** — Node + `jsdom` in a scratch dir; extract the inline `<script>`, run it with stubbed `document`/`navigator`, call the exposed internals (see the replace-at-end-of-IIFE trick: `script.replace(/\}\)\(\);\s*$/, 'globalThis.__api = {...};\n})();')`).
2. **End-to-end in real Chrome** — Node + `puppeteer-core` pointed at `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`, serve the folder with `python3 -m http.server`, simulate a `ClipboardEvent('paste')` with `DataTransfer` containing rich HTML, click Copy, read `navigator.clipboard.read()`, and assert no curly characters (`\u2018\u2019\u201C\u201D\u2013\u2014\u2026`) survive. Grant `clipboard-read`/`clipboard-write` permissions via the browser context.

Known browser notes: the straightening pipeline was verified in headless Chrome; Safari may need the TreeWalker to be created from `root.ownerDocument` (already done — keep it that way).

## Do not

- Do not commit unless asked.
- Do not reintroduce a dark theme: pasted word-processor content often hardcodes `color:#000000` and becomes unreadable on dark backgrounds. The UI is intentionally light-only.
