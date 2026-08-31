# Central Spare Parts Store

A single self-contained inventory manager for a spare parts store — item & rate list, stock ledger, and daily department requirements. Runs entirely in the browser, no server or database required, and works offline once loaded (including on an iPad/tablet).

## Login

The app has a sign-in screen with two built-in accounts:

| Username | Password | Role  |
|----------|----------|-------|
| Ramesh   | 1234     | Staff |
| Admin    | 1234     | Admin |

Staff can use the Item List, Store Record and Daily Requirement tabs. Only Admin can see the **Backup & Restore** tab (data export/import and "erase everything").

**Important:** this is a client-side-only check baked into `index.html` — it keeps casual users out, but it is not real security. Anyone who can view this page's source (including in a public GitHub repo) can read these passwords, and anyone with the browser console can bypass the screen entirely. **Keep this repository private** if that matters to you, and change the passwords in `index.html` (search for `var USERS =`) before relying on it for anything sensitive.

## Features

- **Item & Rate List** — parts catalog: name, specification/category, unit, rate, discount, supplier
- **Store Record** — stock ledger: opening/received/total stock, issued qty, balance, department, rack location (totals and balances auto-calculate)
- **Daily Store Requirement** — department requests: order qty, received, balance (grouped by department)
- **Photos** — take a photo (camera) or upload one for any item/ledger entry
- **Excel import/export** — bring in `.xlsx`/`.csv` files (including multi-sheet workbooks, e.g. one sheet per day, merged in one import), and export any table — or the whole thing — back out as a `.xlsx` workbook
- **Full JSON backup/restore** — a complete backup including photos
- Built for touch: works well on iPad/tablet as well as desktop

## Data storage

All data is saved to `localStorage` in the browser you open this page in. **It is not synced anywhere** — if you clear site data, use private browsing, or switch device/browser, that data won't be there. Use the **Backup & Restore** tab (Admin account) regularly to export a copy (Excel for editing, JSON for a full backup including photos).

## Running it

Just open `index.html` in any modern browser — no build step, no install.

### Hosting on GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, pick your default branch and `/ (root)`.
4. Save — GitHub gives you a URL like `https://<username>.github.io/<repo>/` a minute or two later.

Note: because all data lives in the browser's local storage, each visitor/device keeps its own separate data — a GitHub Pages deployment does not give you shared/synced data across devices or people. And since GitHub Pages sites are public, treat the login as a basic gate, not a real barrier — see the note above.

## Tech

Plain HTML/CSS/JavaScript, no build tooling. Excel reading/writing is done with [SheetJS](https://sheetjs.com/) (bundled inline in `index.html` so the page works fully offline).
