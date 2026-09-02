# AGENTS.md — Single-page web tool conventions

This repository contains a **single-file HTML web app** ("tracker" style): one self-contained
`index.html` with zero external dependencies, usable by opening it locally or hosting it
statically on GitHub Pages. Read this before building, editing, or packaging.

---

## What the tool is

A small personal-data app for a single focused use case. Common shape:

- **Home tab** — add an entry + summary charts.
- **History tab** — filterable list of dated records (presets: This week / This month /
  Last month / Year to date / All, plus custom date range).
- **Registry / Routes tab** — distinct entities (tracked by id) stored once; renaming or
  deleting updates everywhere; expandable rows show the history attached to an entity.

---

## Hard rules (non-negotiable)

1. **One self-contained HTML file, zero external dependencies.** No frameworks, no build
   step, no CDN, no npm, no `<script src="...">`. All markup, CSS, and JS inline in a single
   `index.html`. This is what makes deployment trivial (open the file OR host it statically).
2. **No backend, no servers, no network calls.** The page never transmits data anywhere.
   GitHub just serves the HTML; it never touches user data.
3. **Data is a separate JSON file the user opens and saves.** Browsers can't write to
   local/cloud folders automatically, so the workflow is:
   **open file → track → save file back** (download the updated JSON, replace the old file).
4. **Dark/light theme toggle** out of the box, remembered in `localStorage`.
5. **`index.html` is the filename, always.** Pages serves it automatically, so the site URL
   is just `https://<user>.github.io/<repo>/`.
6. **`index.html` + `README.md` + `LICENSE` (MIT)** per repo.

---

## Proven patterns (reuse these; don't reinvent)

- **Load screen**: file picker (drag/click upload area), a "download a sample" link, and a
  "start from scratch (empty file)" option.
- **JSON load/save/download flow** and a `normalize()` that tolerates older files missing
  newer fields.
- **Data model**: a registry of distinct entities keyed by id + a flat list of dated
  session/entry records referencing the registry by id.
- **Home tab**: an add form that can pick an existing entity (auto-fills details) OR define a
  brand-new one; show recent entries + charts.
- **History tab**: date-range filter with presets.
- **Registry tab**: edit/delete updates everywhere; expandable rows show per-entity history.
- **Charts**: bar charts using `niceTicks` / `formatTick` axis machinery; category charts
  (x = a categorical field) with month/year/all range tabs.
- **Form inputs**: prefer **`<datalist>`-backed text inputs** so users get a dropdown of
  previously-seen/custom values but can still type a new one. Populate from unique prior
  values in the data (helpers like `uniqueValues`, `fillList`).
- **Theme**: dark/light toggle remembered via a `localStorage` key.

**Always ask clarifying questions up front** about the data model (what are the distinct
entities vs. the session records, what fields does each have, what should the chart buckets
be) before writing code.

---

## Data file default name

Make the default data filename predictable (e.g. `app-data.json`) so the README's
"replace with same name" steps stay stable and match the app's actual default.

---

## Session continuity, menu & PWA (know before editing)

- **The app auto-resumes.** After every data mutation/render, `backupDB()` writes a rolling
  latest-state backup to `localStorage` under a `<slug>-data-backup` key (JSON
  `{savedAt, filename, data}`). At script start, `autoStart()` reads it and, if the data
  validates (`bk.data.<key>`), silently calls `loadAndStart()` — so refresh / closed tab just
  picks up where you left off. The **load screen only appears on first-ever use** (no backup
  exists yet). Corrupt backups fall through safely to the load-screen error path.
- **Don't add a "Restore last session" UI.** The backup is a rolling latest-state, so a
  restore would just reload the same data already in memory — it was deliberately removed.
- **Menu instead of inline buttons.** The topbar uses a ☰ hamburger (`#menu-btn` + `#menu`
  with `.menu-item` children; `.open` toggles it open; it closes on outside click or item
  click). Items: **Save backup file…** (`#save-btn`, downloads the JSON), **Open a data
  file…** (`#open-item`, clicks the hidden load-screen `#file-input`), **New file**
  (`#new-file-btn`, confirm + starts empty). Theme toggle stays visible outside the menu.
  Note: **New file keeps the backup** (it becomes the empty DB) so a later refresh still
  resumes with no load screen.
- **`[hidden]` gotcha:** menu items use `display:block`, which overrides the browser's built-in
  `[hidden]` rule — that's why the CSS keeps a global `[hidden], .hidden { display: none !important; }`.
  Any element hidden via the attribute depends on it; `backupDB()` also skips writes while
  `#app` has the `.hidden` class.
- **PWA files are part of the deployable set:** `manifest.json`, `sw.js` (pre-caches `./`,
  the manifest, and the icon; network-first navigations, cache-first assets), and `icon.png`
  (third-party asset — keep the license attribution notes in the README). `index.html` has
  the matching head metas + service-worker registration.

---

## README conventions

- **Title + one-line tagline** — e.g. *"Dead simple X tracker, saves data locally/personal
  iCloud. No ads, no tracking. Single html page."*
- **Usage** — hosted GitHub Pages URL, or open `index.html` locally; note that no data is
  transmitted anywhere and you only open your JSON from local/cloud storage.
- Mention saving to **iCloud** (works across your devices) and that other filesystem
  integrations (Google Drive, etc.) likely work but are untested.
- Walk a non-technical user through: GitHub Pages setup (public repo → push `index.html` →
  Settings → Pages → branch / root), bookmarking the URL, keeping data in iCloud, and the
  iOS/iPadOS workaround (open the file once in the Files app so it's downloaded locally,
  then browse to iCloud Drive, not "Recents").
- Mandatory **WARNING** block noting the tool was vibecoded with the free OpenCode Big
  Pickle AI model + month/year.
- **Screenshots** section with preview images.

---

## Verification (required before declaring done)

There is **no JS runtime / node** in the build environment, so the app can't be executed
for tests. Before considering a tool complete, run static checks:

1. **Brace balance** — count `{` and `}` in the `<script>` block; they must be equal.
2. **Backtick balance** — template literals use backticks; the count must be **even**.
3. **ID integrity** — every `getElementById("id")` must have a matching `id="id"` in the
   HTML.
4. Re-check after every edit (edits can silently unbalance a template literal).

Example:

```sh
awk '/<script>/{f=1;next}/<\/script>/{f=0}f' index.html | grep -o '{' | wc -l   # opens
awk '/<script>/{f=1;next}/<\/script>/{f=0}f' index.html | grep -o '}' | wc -l   # closes (must match)
awk '/<script>/{f=1;next}/<\/script>/{f=0}f' index.html | grep -o '`' | wc -l   # must be even
```

Also do a manual review of the surrounding code for correct logic, since the app can't be
executed.