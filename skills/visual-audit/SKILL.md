---
name: visual-audit
description: Visual audit of a web app at mobile and desktop viewports. Screenshots every reachable screen, reports text overflow, clipping, missing values, contrast issues, and layout problems. Use when the user asks to visually review, audit, or check a web app's UI.
---

# Visual Audit

Systematically screenshot every reachable screen of a web app at mobile viewport sizes, identifying visual polish issues: text overflow, clipping, missing values, broken layout, illegible text, and UI inconsistencies.

## Arguments

`<args>` is a freeform string. Extract:
- **URL** — the app URL to audit (default: detect from project config, or `http://localhost:5174`)
- **Viewports** — screen sizes to test (default: `375x812` iPhone, `360x640` small Android)
- **Focus areas** — specific screens or components to pay extra attention to
- Any other context the user provides (e.g. "sandbox mode", "demo mode", known issues)

## Setup

Use `playwright-cli` for all browser automation:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

If the app isn't running, check for dev server commands in the project's CLAUDE.md, package.json, or similar config. Start servers if needed before beginning the audit.

## Phase 1: Discover Screens

1. Open the app URL at the first viewport size
2. Take a snapshot to find all navigation elements (nav bars, tabs, menus, links)
3. Build a **screen map** — list every reachable screen/view from the navigation:
   - Bottom nav tabs
   - Tab panels within screens
   - Drill-down views (detail pages accessible by clicking cards/items)
   - Modal/sheet overlays
   - Settings sub-screens
4. Plan a traversal order that covers every screen

## Phase 2: Systematic Screenshotting

For each viewport size, visit every screen in the map:

1. Navigate to the screen
2. Take a **viewport screenshot**
3. Take a **snapshot** (accessibility tree) to read text content precisely
4. If the page scrolls, scroll to the bottom and screenshot again
5. If specific elements look problematic, take **element screenshots** for closer inspection
6. Note any console errors (`playwright-cli console`)

### What to Check at Each Screen

| Category | What to Look For |
|----------|-----------------|
| **Clipping** | Content hidden behind nav bars, headers, or other fixed elements. Last items in lists cut off. |
| **Text overflow** | Text breaking mid-word, overlapping adjacent elements, or extending beyond containers |
| **Missing values** | Empty cards, "0" or "NaN" or "undefined" where real data should be, placeholder text in production |
| **Illegible text** | Text below 11px, low contrast against background, text on busy backgrounds |
| **Layout breaks** | Elements stacking incorrectly, unexpected gaps, misaligned columns, broken grids |
| **Scroll issues** | Content unreachable due to overflow:hidden, scrollable areas that don't scroll, content behind sticky headers/footers |
| **Empty states** | Screens with no data that show blank space instead of an empty state message |
| **Inconsistency** | Different spacing, font sizes, or colours used for the same type of element across screens |
| **Touch targets** | Buttons or interactive elements smaller than 44x44px on mobile |

## Phase 3: Compile Findings

After screenshotting all screens at all viewports, compile findings into a structured report.

### Report Format

For each issue found:
- **Screen**: which screen/tab/view
- **Severity**: High (broken/unusable), Medium (confusing/ugly), Low (polish)
- **Description**: what's wrong, with the specific text or element
- **Screenshot evidence**: reference the screenshot that shows the issue
- **Viewport**: which viewport(s) it affects
- **Suggested fix**: brief description of the CSS/code change needed

### Summary Table

Present a summary table at the end:

```
| # | Screen | Issue | Severity | Viewport |
|---|--------|-------|----------|----------|
| 1 | Skills tab | Last card clipped by nav | High | 375, 360 |
| 2 | ... | ... | ... | ... |
```

Group by severity (High first), then by screen.

## Phase 4: Fix Plan

If the user wants fixes:
1. For each issue, identify the specific file and CSS/code location
2. Prioritise: High severity first, then Medium, skip Low unless requested
3. Propose the minimal change needed — prefer CSS fixes over structural changes
4. After applying fixes, re-screenshot the affected screens to verify

## Tips

- **Scroll containers**: Many SPAs use inner scroll containers (not `window`). If `window.scrollTo` doesn't work, find the scrollable element: `document.querySelector('[class*=scroll]')` or check `overflow-y: auto` on parent elements.
- **Fixed overlays**: Bottom navs, headers, and floating buttons often use `position: fixed/absolute`. Check if content has adequate padding-bottom to clear them.
- **Element screenshots**: Use `playwright-cli screenshot <ref>` to zoom into specific elements when the viewport screenshot is too small to judge text clearly.
- **Multiple viewports**: Always test the smallest viewport (360px) — issues that are marginal at 375px often break at 360px.
- **Dark themes**: Pay extra attention to contrast. Dim text on dark backgrounds is a common issue.
- **Sandbox/demo mode**: If the app has a demo or sandbox mode, prefer it for auditing — it provides consistent data without needing auth.
