# Scout Browse

Use `scout-browse` for all browser automation instead of `playwright-cli`. It uses Patchright (anti-detect browser) with a persistent Google profile and NopeCHA CAPTCHA solver — far more capable against anti-bot protections.

## Setup

```bash
source ~/projects/scout-engine/.venv/bin/activate
```

## Commands

```bash
scout-browse open <url>           # open browser and navigate
scout-browse snapshot             # capture page structure + extract embedded JSON
scout-browse click <ref>          # click element by ref from snapshot
scout-browse fill <ref> <text>    # fill input
scout-browse type <text>          # type into focused element
scout-browse goto <url>           # navigate
scout-browse screenshot           # take screenshot
scout-browse close                # close browser
```

## Flags

Flags go **before** the subcommand (e.g. `scout-browse --headless open <url>`):

- `--headless` — run headless (default is headed, which avoids Cloudflare)
- `--no-profile` — don't load Google profile
- `--no-extensions` — don't load NopeCHA
- `--profile <path>` — use custom profile directory

## Pattern

1. `scout-browse open <url>` — opens browser, returns snapshot path
2. Read the snapshot YAML: `cat .scout-browse/page-*.yml`
3. Check for extracted data: `cat .scout-browse/jsonld-*.json` or `cat .scout-browse/next-data-*.json`
4. `scout-browse click <ref>` / `scout-browse fill <ref> <text>` to interact
5. `scout-browse snapshot` to capture updated state

## Output Directory

All output goes to `.scout-browse/` in the current directory:
- `page-{timestamp}.yml` — page accessibility snapshot
- `jsonld-{timestamp}.json` — extracted JSON-LD (schema.org) data
- `next-data-{timestamp}.json` — extracted `__NEXT_DATA__` (Next.js)
- `screenshot-{timestamp}.png` — page screenshots
- `console-{timestamp}.log` — browser console output
- `state.json` — browser connection state (for reconnecting between commands)

## Anti-Bot Stack

- **Patchright** — patches CDP leaks, removes automation detection at browser level
- **Google profile** (`~/.scout/profiles/google`) — persistent cookies, logged-in session
- **NopeCHA** (`~/.scout/extensions/nopecha/`) — auto-solves CAPTCHAs (100 free/day)

## When to Use

- Browsing sites that block WebFetch (Cloudflare, bot detection)
- Research tasks requiring JavaScript rendering
- Inspecting page structure for scraper development
- Any site that needs a real browser session
