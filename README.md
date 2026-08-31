# Central Spare Parts Store

A single self-contained inventory manager for a spare parts store — item & rate list, stock ledger, and daily department requirements. Data is stored in **Supabase** (Postgres + Auth + Storage) and syncs live across every device signed in — add a photo on a phone in the store and it appears instantly on the office iPad. Branded with the Rugs Creation logo, embedded directly in the login screen.

## Login

Real accounts, backed by Supabase Auth:

| Username | Password | Role  |
|----------|----------|-------|
| Ramesh   | 1234     | Staff |
| Admin    | 1234     | Admin |

Staff can use the Item List, Store Record and Daily Requirement tabs, and can add/edit/delete any row. Only Admin can see the **Backup & Restore** tab (Excel/JSON export, restore, and "erase everything").

**Please change these passwords** once you're live — either from a Supabase SQL query against `auth.users`, or by adding a "change password" flow later. Unlike the previous version of this app, these are real server-checked accounts (Supabase Auth + Row Level Security), not a check baked into the page — so it's safe to keep this repo public and host it on GitHub Pages.

## Features

- **Item & Rate List** — parts catalog: name, specification/category, unit, rate, discount, supplier
- **Store Record** — stock ledger: opening/received/total stock, issued qty, balance, department, rack location (totals and balances auto-calculate)
- **Daily Store Requirement** — department requests: order qty, received, balance (grouped by department)
- **Photos** — take a photo (camera) or upload one for any item/ledger entry; stored in Supabase Storage
- **Live sync** — every signed-in device sees changes within a second or two, no refresh needed
- **Excel import/export** — bring in `.xlsx`/`.csv` files (including multi-sheet workbooks, e.g. one sheet per day, merged in one import), and export any table — or the whole thing — back out as a `.xlsx` workbook
- **Full JSON backup/restore** — a complete backup including photo URLs
- Built for touch: works well on phone, iPad/tablet and desktop

## Data storage & architecture

All data lives in a Supabase project (Postgres database), not in the browser. `index.html` connects to it using a Supabase **project URL** and **anon (public) key**, both of which are safe to ship in client-side code — access is enforced server-side by Row Level Security policies (only signed-in accounts can read/write; only Admin can manage other accounts). Photos are stored in a Supabase Storage bucket.

Nothing is stored in `localStorage` anymore except your last-used tab. As long as your device has an internet connection, you're looking at live, shared data.

## Running it

Just open `index.html` in any modern browser — no build step, no install. An internet connection is required (the app talks to Supabase for every read/write).

### Hosting on GitHub Pages

1. Push this repo to GitHub (public or private — either is fine now).
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, pick your default branch and `/ (root)`.
4. Save — GitHub gives you a URL like `https://<username>.github.io/<repo>/` a minute or two later.

If you connected this repository to Supabase's GitHub integration, that integration manages *database migrations* (schema changes) when you push SQL migration files — it does not change or rebuild `index.html`. The two accounts, tables, storage bucket and security policies already exist on the Supabase project this file points to, so no extra setup is needed there; just push and enable Pages as above.

### Changing which Supabase project it talks to

Open `index.html`, search for `SUPABASE_URL` and `SUPABASE_ANON_KEY` near the top of the second `<script>` block, and replace both with the values from your own project (**Project Settings → API**). You'll also need to recreate the `csps_items`, `csps_ledger`, `csps_requirements`, `csps_profiles` tables, the storage bucket, the Row Level Security policies, and the two accounts — ask whoever set up the original project for the schema, or for the SQL that created it.

## Tech

Plain HTML/CSS/JavaScript, no build tooling. The [Supabase JS client](https://supabase.com/docs/reference/javascript) and [SheetJS](https://sheetjs.com/) (for Excel reading/writing) are both bundled inline in `index.html` so the only network calls the page makes are to your Supabase project — nothing else to install or host. The Rugs Creation logo is embedded as base64 image data directly in `index.html`.
