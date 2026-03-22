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
- [ ] Deploy to GitHub Pages (so it works on phone without Live Server)
- [ ] PWA service worker (offline support on London Underground)
- [ ] Bilingual EN/FR toggle
- [ ] Fix bad geocoded coordinates in the itinerary sheet (any lat ~45.5, lng ~-73.5 = Montreal, not UK)
- [ ] Update `GBP_TO_CAD` closer to the trip date
