# Scout Browse

Use `scout-browse` for all browser automation instead of `playwright-cli`. It uses Patchright (anti-detect browser) with a persistent Google profile and NopeCHA CAPTCHA solver — far more capable against anti-bot protections.

## Commands

```bash
# Core
scout-browse open <url>           # launch browser and navigate (names tab from domain)
scout-browse open <url> --name x  # launch with a custom tab name
scout-browse snapshot             # capture page structure + extract embedded JSON
scout-browse click <ref>          # click element by ref from snapshot
scout-browse fill <ref> <text>    # fill input
scout-browse type <text>          # type into focused element
scout-browse goto <url>           # navigate current tab
scout-browse resize <w> <h>      # set viewport size (e.g. 1440 900)
scout-browse scroll <target>     # up, down, top, bottom, or pixel amount
scout-browse screenshot           # take screenshot
scout-browse close                # close browser

# Tab management
scout-browse tab open <url>             # open new tab (auto-named from domain)
scout-browse tab open <url> --name x    # open new tab with custom name
scout-browse tab list                   # list all tabs (* = active)
scout-browse tab select <name>          # switch active tab
scout-browse tab close <name>           # close a tab
```

## Flags

Global flags go **before** the subcommand (e.g. `scout-browse --headless open <url>`):

- `--headless` — run headless (default is headed, which avoids Cloudflare)
- `--no-profile` — don't load Google profile
- `--no-extensions` — don't load NopeCHA
- `--profile <path>` — use custom profile directory

Per-command `--tab <name>` targets a specific tab without switching the active tab:

```bash
scout-browse snapshot --tab zoopla    # snapshot a specific tab
scout-browse click e3 --tab openrent  # click in a specific tab
```

## Pattern — Single Tab

1. `scout-browse open <url>` — opens browser, returns snapshot path
2. Read the snapshot YAML: `cat .scout-browse/page-*.yml`
3. Check for extracted data: `cat .scout-browse/jsonld-*.json` or `cat .scout-browse/next-data-*.json`
4. `scout-browse click <ref>` / `scout-browse fill <ref> <text>` to interact
5. `scout-browse snapshot` to capture updated state

## Pattern — Multiple Tabs

1. `scout-browse open <url>` — launch browser with first tab
2. `scout-browse tab open <url2> --name research` — open second tab
3. `scout-browse tab list` — see all tabs and which is active
4. `scout-browse tab select research` — switch to a tab
5. `scout-browse snapshot` — snapshot targets the active tab
6. `scout-browse click e3 --tab research` — interact with a specific tab without switching

Snapshot YAML headers include `tab: <name>` so you know which tab each snapshot belongs to.

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
