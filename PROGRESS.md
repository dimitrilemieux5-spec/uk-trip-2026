# UK Trip 2026 — App Progress Log

---

## Session 1 — 2026-03-17

### What we built
Single-file travel companion web app (`index.html`) — vanilla HTML/CSS/JS, no frameworks, reads all data from Google Sheets published as CSV.

**Data sources (4 sheets, same spreadsheet):**
| Sheet | Purpose | gid |
|---|---|---|
| itinerary | Day-by-day stops | 0 |
| places | Full place list with priorities | 226013303 |
| checklist | Pre-trip task list | 195264691 |
| budget_categories | Budget by category (CAD) | 142940218 |

**Features shipped:**
- Robust CSV parser (handles quoted fields, embedded commas)
- 3-tab layout: **Itinerary**, **Checklist**, **Budget** — tab bar sticky below header
- **Itinerary tab**: all 17 days (July 4–20), each as a card with date, city badge (navy = London, green = Edinburgh), day theme, and stop list (stop name + category pill + cost badge)
- **Cost badges**: FREE / $ / $$ / $$$ color-coded
- **Category pills**: food, transport, landmark, museum, shopping, music
- **Expandable stop notes**: stops with a `stop_note_en` value get a `▾` chevron — click to reveal
- **Google Maps embed** per day card: numbered pins, auto-fitted bounds — scaffolding in place, waiting on API key + lat/lng data
- **Checklist tab**: grouped by category, items show `done` / `urgent` / `pending` status badges and notes
- **Budget tab**: table with proportional bar chart per category, bold total row
- Mobile-first, readable at 375px

### Pending (to do next session)
- [ ] Get Google Maps JavaScript API key from Google Cloud Console
- [ ] Add `lat` and `lng` columns to itinerary Google Sheet for each stop
- [ ] Paste API key into `index.html` line: `const GOOGLE_MAPS_API_KEY = 'YOUR_API_KEY_HERE';`

---

<!-- Add new sessions below this line -->

## Session 2 — 2026-03-18

### What we did
Completed all prerequisites and shipped the budget expense tracker.

**Google Cloud / Maps setup (user completed):**
- Created Google Cloud project "UK Trip 2026"
- Enabled Maps JavaScript API
- Generated API key: live and working in `index.html` (line ~405)
- Set $0 budget cap with 50/90/100% email alerts

**Itinerary sheet — lat/lng geocoding:**
- Added `lat` and `lng` columns to the itinerary Google Sheet
- Wrote a Google Apps Script (`geocodeStops()`) using `Maps.newGeocoder()` to auto-fill coordinates for all stops
- Workaround used: `lngCol = latCol + 1` (the `lng` header had a hidden character; header was later fixed to `lng`)
- All rows now have coordinates; a few vague stops (e.g. "Walk around neighbourhood") may need manual review

**Features shipped this session:**

- **Google Maps embedded per day card** — fully live. Numbered markers, auto-fitted bounds. Days with no coordinates show no map (graceful skip).
- **Budget tab → full expense tracker:**
  - Top summary bar: Budgeted / Spent / Remaining (CAD)
  - Per-category cards with colour-coded progress bars (green→amber→orange→red at 60/80/100%)
  - `+ Add Expense` FAB (floating button, bottom-right, only visible on Budget tab)
  - Modal slide-up form: GBP amount + live CAD preview, category dropdown, optional note, date
  - Recent expenses list (newest first) with 🗑 delete button per row
  - All expenses stored in `localStorage` key `uk_trip_expenses` — persists across reloads, works offline
  - Exchange rate: `const GBP_TO_CAD = 1.72` (update before trip)

### App state: fully functional ✅
All 5 planned features are working: Itinerary (17 days + maps), Checklist, Budget tracker.

### Remaining nice-to-haves (future sessions)
- [ ] Deploy to GitHub Pages (so it works on phone without Live Server)
- [ ] PWA service worker (offline support on London Underground)
- [ ] Bilingual EN/FR toggle
- [ ] Verify geocoded coordinates look correct on the maps (spot-check a few days)
- [ ] Update `GBP_TO_CAD` closer to the trip date

---

## Session 3 — 2026-03-18

### What we built

**Expense tracker improvements:**
- **Currency toggle (GBP / CAD)** on the Add Expense modal — enter pounds for daily spending, CAD for pre-paid items (flights, Airbnb). Expense list shows whichever currency was entered as primary, with the other in brackets.
- **Edit expense** — ✏ button on each expense row re-opens modal pre-filled; "Save" becomes "Update". Fixed a bug where the FAB (`+`) was passing the click event as an argument, causing it to open in edit mode incorrectly.

**Itinerary — stop timestamps:**
- Added `stop_start` and `stop_end` columns to the Google Sheet (column order doesn't matter — parser reads by header name)
- Time range displayed on each stop row: `09:30–12:00`, or just `09:30` if only start is set
- Time styled in main text colour for readability

**Map enhancements:**
- **Clickable pins** — tapping a map pin opens Google Maps search for that stop by name + city (e.g. `"Natural History Museum, London, UK"`), reliably finding the correct place regardless of coordinate accuracy
- **Directions links between stops** — `→ directions` link appears between each consecutive stop pair; opens Google Maps with named origin + destination (name-based, not coordinate-based, to avoid reverse-geocoding to wrong addresses). Styled in navy blue, bold, more visible.
- **Known data issue**: geocoded lat/lng in the sheet are sometimes wrong (Montreal-area coordinates for some stops). Embedded map pin positions are affected. Pin clicks and directions links are unaffected (use name search). User should manually review/correct lat/lng values in the sheet where maps look wrong.

### Google Sheet columns in use (itinerary tab)
`date`, `city`, `day_number`, `day_theme`, `stop_order`, `stop_name`, `stop_start`, `stop_end`, `stop_category`, `stop_cost`, `stop_note_en`, `maps_url`, `lat`, `lng`

### Remaining nice-to-haves
- [x] Deploy to GitHub Pages ✅
- [ ] PWA service worker (offline support on London Underground)
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet (any lat ~45.5, lng ~-73.5 = Montreal, not UK)
- [ ] Update `GBP_TO_CAD` closer to the trip date
- [ ] Add Airbnb pins to day card maps (plan ready — purple home marker, London + Edinburgh)

---

## Session 4 — 2026-03-22

### What we did

**Map widget:**
- Increased map height from 220px to 320px for better visibility

**GitHub Pages deployment:**
- Installed GitHub CLI (`gh`)
- Created GitHub account (`dimitrilemieux5-spec`)
- Initialized git repo, created `.gitignore` (excludes `.claude/`)
- Created public repo `dimitrilemieux5-spec/uk-trip-2026` and pushed code
- Enabled GitHub Pages on `master` branch

**Live URL:** https://dimitrilemieux5-spec.github.io/uk-trip-2026/

### Remaining nice-to-haves
- [x] PWA service worker ✅
- [x] Add Airbnb pins to day card maps ✅
- [x] Restrict Maps API key to GitHub Pages domain ✅
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet (any lat ~45.5, lng ~-73.5 = Montreal, not UK)
- [ ] Update `GBP_TO_CAD` closer to the trip date

---

## Session 5 — 2026-03-22

### What we built

**Airbnb pins on day card maps:**
- Purple circle marker (⌂) on every day card map
- London pin: 51.50673285748828, -0.2280735424 (9A Stanlake Villas, Shepherd's Bush)
- Edinburgh pin: 55.939544, -3.2163927 (6 McNeil St, EH11 1JN)
- Clicking pin opens Google Maps to the Airbnb address
- Pin always included in map bounds

**PWA / offline support:**
- Created `sw.js` service worker — caches app shell on install, caches Google Sheets CSV data on first load (network-first with cache fallback)
- Created `manifest.json` — app name, theme color, standalone display
- Registered service worker in `index.html`
- App now works offline using last-cached Sheet data
- Can be added to phone home screen via browser Share menu

**Maps API key restricted** to `dimitrilemieux5-spec.github.io/*`, `localhost:5500/*`, `127.0.0.1:5500/*`

### Remaining nice-to-haves
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet (any lat ~45.5, lng ~-73.5 = Montreal, not UK)
- [ ] Update `GBP_TO_CAD` closer to the trip date
- [ ] Add PWA icons (192×192 and 512×512) for proper "Add to Home Screen" prompt on all browsers

---

## Session 6 — 2026-03-22

### What we built

**Checklist tab — per-person status tracking:**
- Each checklist item now has two independent status dropdowns: **D** (Dimitri) and **C** (Charlotte)
- Status options: To Do / In Progress / Complete / Abandoned — colour-coded (grey / blue / green / strikethrough grey)
- Both statuses save independently to `localStorage` (key: `uk_trip_checklist_state`)
- Item text strikes through only if **both** statuses are set to Abandoned
- Default status derived from the Sheet's existing `status` column (`done` → Complete, everything else → To Do)
- No Sheet changes required

**Budget tab — per-person expense tracking:**
- **Dimitri / Charlotte toggle** at the top of the Budget tab — each person sees their own expenses and budget progress independently
- Each person tracks against the full $7,500 per-person budget breakdown from the Sheet
- **Who dropdown** added to Add/Edit Expense modal (Dimitri / Charlotte, defaults to Dimitri)
- Small **D / C badge** on each expense row in the recent expenses list
- Old expenses with no `who` tag default to Dimitri

**PWA icons:**
- Added `icon-192.png` and `icon-512.png` (custom image — a very distinguished British cat)
- Updated `manifest.json` to reference both icons
- Added `apple-touch-icon` meta tag to `index.html` for iOS home screen support
- App now shows a proper icon when added to phone home screen

### Remaining nice-to-haves
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet (any lat ~45.5, lng ~-73.5 = Montreal, not UK)
- [ ] Update `GBP_TO_CAD` closer to the trip date

---

## Session 8 — 2026-03-22

### What we fixed

**localStorage persistence (service worker bug):**
- Data (checklist statuses, budget expenses) was not persisting across app restarts because the service worker was serving a stale cached `index.html` from before those features were added
- Bumped cache to `uk-trip-v2` to force a fresh install — caused a new iOS Safari error ("Returned response is null") because the empty cache returned `null` on Google Sheets misses
- Final fix: removed Google Sheets caching from the service worker entirely, bumped to `uk-trip-v3`. App shell still cached offline; Sheet data requires internet. localStorage data (expenses, checklist statuses) always works offline.

### Remaining nice-to-haves
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet (any lat ~45.5, lng ~-73.5 = Montreal, not UK)
- [ ] Update `GBP_TO_CAD` closer to the trip date

---

## Session 9 — 2026-03-22

### What we built

**Itinerary tab — search + category filter:**
- Search bar at the top of the Itinerary tab: case-insensitive substring match across stop names, themes, categories, and notes
- Matching stop names highlighted in yellow (`<mark>`)
- ✕ clear button appears while typing
- "No stops match your search." empty state
- Category filter bar: **All · Food · Transport · Landmark · Museum · Shopping · Music** — filters day cards to only those containing at least one stop of the selected category
- Search and category filter work together (both apply simultaneously)

**Map tab (new):**
- New **Map** tab added between Itinerary and Checklist
- Fetches `places` sheet (gid=226013303) with columns: `city`, `name`, `category`, `maps_url`, `priority`, `note_en`, `note_fr`, `lat`, `lng`
- Full Google Map with color-coded circle markers by category: food=orange, transport=blue, landmark=green, museum=pink, shopping=purple, music=red, other=gray
- **City toggle**: London / Edinburgh / All — filters markers and refits map bounds
- **Search bar**: filters by name, category, note, city
- **Category filter bar**: same categories as itinerary tab
- **Color legend** below controls
- **Scrollable place list** below the map — each row shows colored dot, name, category pill, LDN/EDI badge
- Tapping a pin or a list row opens Google Maps searching `"Place Name, London, UK"` (or Edinburgh) — identical behavior to the itinerary tab pins
- Fixed bug: `cityKey` was computed after `cityLabel` used it, so all places defaulted to London search; corrected variable order

### Remaining nice-to-haves
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet and places sheet (lat ~45.5, lng ~-73.5 = Montreal)
- [ ] Update `GBP_TO_CAD` closer to the trip date
- [ ] Populate the places sheet with more Edinburgh landmarks, restaurants, and activities

---

## Session 10 — 2026-04-05

### What we fixed

**Map tab — place list now filters with city/category buttons:**
- Clicking London/Edinburgh/All or any category pill (Food, Transport, etc.) now correctly hides/shows both map pins AND the place list rows simultaneously
- Root cause: CSS specificity bug — `.place-row { display: flex }` overrode the browser's built-in `[hidden] { display: none }`, so `row.hidden = true` had no visual effect despite the filter logic being correct
- Fix: added `.place-row[hidden] { display: none; }` to `index.html`
- Also bumped service worker cache from `uk-trip-v8` → `uk-trip-v9` to force browsers to fetch the updated `index.html` (old cached version was still being served after the push)

### Remaining nice-to-haves
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet and places sheet (lat ~45.5, lng ~-73.5 = Montreal)
- [ ] Update `GBP_TO_CAD` closer to the trip date
- [ ] Populate the places sheet with more Edinburgh landmarks, restaurants, and activities

---

## Session 11 — 2026-04-06

### What we built

**Today tab (new 5th tab) — day-builder feature:**

The core idea: the existing Itinerary tab is a rigid pre-planned schedule, but in practice the users want to choose activities each morning with flexibility, while still having booked/fixed events (Bath day trip, Harry Potter Studios, shows) appear automatically.

**New "Today" tab:**
- Date picker at the top — defaults to today's real date, user picks manually each morning
- **Fixed Events** section: auto-populated from the itinerary sheet for the selected date, for rows where `is_fixed = 1`. Shown with 🔒, sorted by start time, no remove button. Requires user to add `is_fixed` column to itinerary Google Sheet and set `1` on locked events.
- **My Places** section: places added from the Map tab, stored in localStorage keyed by date string (`uk_trip_today_plans`). Each row has a ✕ remove button. Persists across reloads.
- **Route map**: numbered Google Maps markers for all stops (fixed + flexible combined), `fitBounds()` to show the full day. Hidden when no stops have coordinates.

**Map tab additions:**
- Tapping a **map pin** now opens a Google Maps InfoWindow (instead of immediately opening Google Maps) with the place name, an "Open in Google Maps" link, and an **"Add to Today"** button
- Tapping a **list row** toggles an inline action bar below it with the same two options — tapping again closes it
- "Add to Today" saves the place to localStorage for the date currently selected in Today tab, briefly shows "Added ✓", and live-updates the Today tab if open

**Architecture decisions:**
- Kept the existing Itinerary tab untouched — Today tab is purely additive
- Fixed events use the existing itinerary CSV (no new sheet needed), just a new `is_fixed` column
- Flexible places use a new localStorage key `uk_trip_today_plans` — object keyed by `YYYY-MM-DD` date strings, each value is an array of `{ name, category, lat, lng, city, mapsUrl }` objects
- Today tab exposes `_renderForDate(dateStr)` and `_getActiveDate()` on its section element so Map tab can call back into it without global state

**Technical changes:**
- `groupByDay()` — added `is_fixed` field to each stop object (parsed from Sheet column)
- Added 4 localStorage helpers: `getTodayPlans`, `getTodayPlanForDate`, `addPlaceToTodayPlan`, `removePlaceFromTodayPlan`
- New `renderTodayTab(dayMap)` function
- `renderMapTab()` modified: marker click → InfoWindow with "Add to Today"; list row click → toggling `.place-row-actions` action bar
- Service worker bumped `uk-trip-v9` → `uk-trip-v10`

### User must do manually (one-time Sheet setup)
Add an `is_fixed` column to the itinerary Google Sheet tab. Set `1` for Bath day trip, Harry Potter Studios, any booked shows. Leave blank or `0` for all other stops.

### Remaining nice-to-haves
- [x] Push to GitHub ✅
- [ ] Add `is_fixed` column to itinerary sheet and mark locked events
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet and places sheet (lat ~45.5, lng ~-73.5 = Montreal)
- [ ] Update `GBP_TO_CAD` closer to the trip date
- [ ] Populate the places sheet with more Edinburgh landmarks, restaurants, and activities

---

## Session 12 — 2026-04-06

### What we built

**Day Plans tab — expanded from the "Today" tab:**

The Today tab was renamed to **Day Plans** and significantly expanded so plans can be built for any trip day, not just today.

**Tab rename:**
- Tab label: "Today" → "Day Plans" (section element ID `view-today` unchanged — used internally)

**"+ Today" / "+ Other Day" in Map tab:**
- The single "Add to Today" button on map pin InfoWindows and list row action bars is replaced by two buttons side-by-side:
  - **+ Today** — instant add to the currently selected Day Plans date (same behaviour as before)
  - **+ Other Day** — opens a slide-up modal with a date picker (constrained to July 4–20, 2026) plus optional Start and End time fields, then an Add / Cancel button pair
- Modal date defaults to whichever date is active in Day Plans tab

**Reorder flexible places (↑ ↓ buttons):**
- Each flexible place row in Day Plans now has ↑ and ↓ arrow buttons on the left
- First row hides ↑, last row hides ↓
- Clicking reorders the array in localStorage (`uk_trip_today_plans`) and re-renders; route map marker numbers update automatically to match new order

**Optional start/end times on flexible places:**
- Each flexible place row has a ✏ button that toggles an inline editor below the row
- Editor has Start time and End time inputs (`<input type="time">`) pre-filled with existing values, and a Save button
- `updatePlaceInTodayPlan(dateStr, index, patch)` helper patches the object in localStorage
- Times display on the row in the same format as fixed events: `09:30–12:00`, or just `09:30` if only start is set
- Times can also be set at add-time via the "+ Other Day" modal

**New localStorage helpers:**
- `reorderPlaceInDayPlan(dateStr, fromIndex, toIndex)` — splices array in place
- `updatePlaceInTodayPlan(dateStr, index, patch)` — Object.assign patch on one item

**localStorage data structure update** (backwards-compatible — old entries without time fields just show no time):
```js
{
  "YYYY-MM-DD": [
    { name, category, lat, lng, city, mapsUrl, startTime, endTime },
    ...
  ]
}
```

- Service worker bumped `uk-trip-v10` → `uk-trip-v11`

### Remaining nice-to-haves
- [ ] Add `is_fixed` column to itinerary sheet and mark locked events
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet and places sheet (lat ~45.5, lng ~-73.5 = Montreal)
- [ ] Update `GBP_TO_CAD` closer to the trip date
- [ ] Populate the places sheet with more Edinburgh landmarks, restaurants, and activities

---

## Session 13 — 2026-04-06 (same day as Session 12)

### What we built

**Firebase Realtime Database sync — all user data now shared between phones:**

Replaced all `localStorage` with Firebase Realtime Database (free Spark plan) so Dimitri and Charlotte see each other's changes in real-time (~1 second, no reload needed).

**Firebase setup (user completed manually):**
- Created Firebase project "UK Trip 2026" (Spark plan — free, no credit card)
- Created Realtime Database in us-central1, test mode, rules set to permanently open (`".read": true, ".write": true`)
- Registered web app, copied `firebaseConfig`

**Technical changes:**
- Added Firebase compat SDK v10.12.0 via CDN (`firebase-app-compat.js` + `firebase-database-compat.js`)
- Initialized Firebase with project config at top of script
- `fbCache` object holds in-memory snapshots: `{ checklist: {}, expenses: [], dayPlans: {} }`
- Three Firebase listeners (`db.ref('checklist/expenses/day_plans').on('value', ...)`) keep cache live and re-render the active tab when the other phone makes a change
- All read helpers (`getChecklistState`, `getExpenses`, `getTodayPlanForDate`) now read from `fbCache` synchronously
- All write helpers now call `db.ref(...).set(newValue)` instead of `localStorage.setItem`
- Service worker bumped `uk-trip-v11` → `uk-trip-v12`

**Firebase paths:**
| Data | Firebase path |
|---|---|
| Checklist statuses | `/checklist` |
| Budget expenses | `/expenses` |
| Day Plans | `/day_plans` |

### Remaining nice-to-haves
- [ ] Add `is_fixed` column to itinerary sheet and mark locked events
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet and places sheet
- [ ] Update `GBP_TO_CAD` closer to the trip date
- [ ] Populate the places sheet with more Edinburgh landmarks, restaurants, and activities
- [ ] Firebase security (currently open rules — low risk for trip data, revisit post-trip if desired)

---

## Session 14 — 2026-04-06

### What we built

**Merged Day Plans into Itinerary tab:**

Removed the separate "Day Plans" tab and folded its flexible place-building features directly into each day card on the Itinerary tab.

**Changes:**
- **Day Plans tab removed** — tab bar now has 4 tabs: Itinerary, Map, Checklist, Budget
- **Each day card now has a "My Places" section** at the bottom, below the fixed sheet stops. Shows all flexible places added for that date from Firebase, with the same ↑↓ reorder, ✏ edit-time inline editor, and ✕ remove controls that were on Day Plans
- **Flexible place markers on day card maps** — orange circle markers (numbered after fixed stops) appear on each day's embedded Google Map when places are added. Map bounds auto-expand to include them
- **Firebase listener updated** — when any day_plans data changes, re-renders My Places section for all day cards currently in DOM (works across both phones in real-time)
- **Map tab buttons simplified** — replaced "+ Today" / "+ Other Day" two-button pair with a single **"+ Add to Day"** button that opens the date picker modal
- **Date picker modal** — now defaults to today's date (or trip start if before July 4). Firebase listener handles re-rendering automatically after add; no manual callback needed
- Service worker bumped `uk-trip-v12` → `uk-trip-v13`

### Remaining nice-to-haves
- [ ] Add `is_fixed` column to itinerary sheet and mark locked events
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet and places sheet
- [ ] Update `GBP_TO_CAD` closer to the trip date
- [ ] Populate the places sheet with more Edinburgh landmarks, restaurants, and activities
- [ ] Firebase security (currently open rules — low risk for trip data, revisit post-trip if desired)

---

## Session 15 — 2026-04-07

### What we built

**Removed Google Sheets itinerary dependency — itinerary is now fully user-built:**

The itinerary tab no longer fetches or displays data from the Google Sheet. Day cards are generated entirely from code (a hardcoded 19-day date range), and the full itinerary is built by adding places from the Map tab via "+ Add to Day".

**Changes:**
- **Day cards generated from code** — replaced `groupByDay(itinRows)` (sheet-driven) with `generateTripDays()` which produces 19 day objects for July 3–21, 2026. City assignment: July 11–18 = Edinburgh, all other days = London
- **Itinerary CSV fetch removed** — `init()` no longer fetches `ITINERARY_URL`. Only 3 sheet fetches remain: checklist, budget, places
- **Day card simplified** — removed day theme, fixed stop list, and direction links between sheet stops. Each card now shows only: day number, city badge, date, embedded map, and "My Places" section
- **Map always shown** — all 19 days get an embedded map centered on the relevant Airbnb pin initially
- **Search/filter bar removed** — was only useful for searching sheet stops
- **Header date** updated from "July 4–20" to "July 3–21"
- **Directions in My Places** — each place in the "My Places" list now has a "→ directions" link below it. First place uses the Airbnb as origin ("→ directions from Airbnb"); subsequent places use the previous place as origin. Origins update automatically on reorder
- Service worker bumped `uk-trip-v13` → `uk-trip-v14`

### Known data issue (fix next session)
Some places in the `places_london` sheet (e.g. Raven Records) have wrong lat/lng coordinates pointing to the Airbnb area instead of the actual location. Directions links use stored lat/lng, so they route incorrectly for those places. Fix: look up correct coordinates on Google Maps, right-click the pin → "Copy coordinates", and update the row in the sheet.

### Remaining nice-to-haves
- [ ] Fix wrong coordinates for affected places in the places sheet (e.g. Raven Records)
- [ ] Bilingual EN/FR toggle
- [ ] Update `GBP_TO_CAD` closer to the trip date
- [ ] Populate the places sheet with more Edinburgh landmarks, restaurants, and activities
- [ ] Firebase security (currently open rules — low risk for trip data, revisit post-trip if desired)
