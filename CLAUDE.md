# UK Trip Planner — Project Brief
### Claude Code Learning Project · Version 2.0 · March 2026

---

## 1. Project Overview

**What is this?**
A personal, bilingual (English/French) web application for a 16-day UK trip in July 2026. Built incrementally with Claude Code in VS Code, deployed free on GitHub Pages, accessible on desktop and mobile including offline.

**Who is it for?**
Two travellers (a couple) from Montreal, Quebec. Personal use only.

**Design philosophy:** Clean and functional. Mobile-first. Bilingual EN/FR toggle. Works offline.

---

## 2. Trip Facts

| Detail | Info |
|---|---|
| Departure | July 3, 2026 — Montreal (YUL), 10:05 PM |
| Arrival in London | July 4, 2026 — London Gatwick (LGW), 9:50 AM |
| London Airbnb | July 4 check-in to July 11 check-out (by 11 AM) — 9A Stanlake Villas, Shepherd's Bush |
| London to Edinburgh | July 11, 2026 — Train (book ASAP on LNER or Trainline) |
| Edinburgh Airbnb | July 11 check-in to July 19 check-out — Fountainbridge area |
| Edinburgh to London | July 19, 2026 — Train south toward Gatwick (hotel TBD near LGW) |
| Return flight | July 20, 2026 — London Gatwick to Montreal |
| London days | 7 full days (July 4-10) + July 11 morning |
| Edinburgh days | 8 full days (July 11-18) + July 19 morning |

**July 19 hotel:** Bloc Hotel (inside South Terminal, steps from security), Hampton by Hilton (North Terminal, good value), or Travelodge Horley (cheapest). Book soon.

**Day trips (both definite, dates flexible July 6-10):**

- Bath full day trip
- Harry Potter Studio Tour, Leavesden — book transport + entry package at wbstudiotour.co.uk NOW (sells out)

---

## 3. Budget Summary (Per Person)

| Category | Budget (CAD) | Status |
|---|---|---|
| Flights | $1,500 | Booked |
| London accommodation | $1,000 | Booked |
| Edinburgh accommodation | $1,000 | TBD |
| Food and drinks | $1,900 | Flexible |
| Activities | $1,500 | Flexible |
| London-Edinburgh train | $150 | Book ASAP |
| Edinburgh-London train | $150 | Book ASAP |
| Bath day trip | $300 | TBD |
| Total | $7,500 | |

---

## 4. Architecture: Google Sheets as Data Backend

### Why This Architecture

Trip plans change constantly. If itinerary data is baked into the code, every change requires editing code and redeploying. For a beginner, that friction kills motivation.

The solution: **the app code never changes. Only a Google Sheet does.**

### How It Works

You edit a row in a Google Sheet. The Sheet is published as a free CSV URL (no API key, no authentication). The app fetches that CSV on load and displays the latest version of your plan. You change your mind about Day 4 dinner — edit the Sheet, reload the app. Done. No terminal, no code, no redeployment.

### The Golden Rule

**The Google Sheet is the source of truth. The app is just a display layer.**

If something looks wrong in the app, fix the Sheet — never the code. This is how real professional apps work (content management systems, data-driven dashboards). You are learning a legitimate industry pattern.

### The Five Sheet Tabs

| Tab name | What it contains | How often it changes |
|---|---|---|
| config | Trip dates, budget amounts, exchange rate | Set once |
| itinerary | All days, stops, categories, cost flags, notes | Frequently |
| places_london | London saved places with coordinates and category | Rarely |
| places_edinburgh | Edinburgh saved places with coordinates and category | As you add more |
| checklist | Admin tasks and packing items | As you complete things |

### What Stays in the Code (Not the Sheet)

These things are permanent and never need to change:

- Visual design and layout
- Bilingual label translations (EN/FR)
- Budget expense log (localStorage — personal live data)
- Google Maps rendering logic
- PWA offline caching

### Offline Behaviour

When the app loads with internet, it fetches fresh Sheet data and caches it locally. Opening the app offline (e.g., London Underground) uses the last cached version, which is always recent enough. The budget tracker always works offline.

---

## 5. App Features (5 Sections)

### Section 1 — Overview / Dashboard

Data source: config tab.

- Trip name: "London and Edinburgh — July 2026"
- Countdown: days until departure (live calculation)
- Current phase: pre-trip / London week / Edinburgh week / travel day / back home
- Total budget remaining: budgeted total minus all logged expenses
- Quick navigation to all 4 other sections

### Section 2 — Itinerary

Data source: itinerary tab. Toggle between London and Edinburgh.

Sheet columns: date, city, day_number, day_theme, stop_order, place_name, category, cost_flag, note, is_day_trip.

Each day card shows: date, theme, stops in order with category icon and cost flag (FREE / $ / $$ / $$$). Special treatment for day trips and travel days.

To update: edit a Sheet row. App reflects it on next load.

### Section 3 — Map

Data sources: places_london and places_edinburgh tabs.

Sheet columns: name, category, lat, lng, cost_flag, note, maps_url.

Features: colour-coded pins by category (orange=food, blue=museums, green=parks/landmarks, purple=shopping, red=music, white=other), category filter buttons, city toggle, and a "today's route" mode that shows the current day's stops in order.

Google Maps API: free tier is 10,000 map loads per month — far more than a personal app needs. Requires a Google account with credit card on file, but a $0 hard budget cap in Google Cloud Console guarantees zero charges.

To add a place: add a row to the places tab with coordinates. No code changes.

### Section 4 — Budget Tracker

Does NOT use Google Sheets. Expenses are personal live data stored in localStorage. Budget amounts come from the config tab.

Categories: Food and drinks, Transport, Activities, Accommodation, Shopping, Miscellaneous.

Each category shows: budgeted amount, spent so far, remaining, and a progress bar that goes from green to yellow to red.

Adding an expense (3 taps on mobile): tap +, enter amount and category, tap Save.

Currency: GBP with CAD equivalent using a fixed exchange rate from the config tab.

### Section 5 — Checklist

Data source: checklist tab. Checked state lives in localStorage.

Sheet columns: item_en, item_fr, category, priority, note, default_done.

Pre-loaded items include: passport (done), travel insurance (TBD), contact bank, UK ETA (urgent, 15 GBP each at gov.uk/eta), both trains (urgent), Gatwick hotel, Harry Potter Studios (urgent), AirTags, walking shoes, UK plug adapters, power bank.

To add an item: add a row to the checklist tab. No code changes.

---

## 6. Technical Stack

| Layer | Tool | Cost |
|---|---|---|
| Code editor | VS Code | Free |
| AI assistant | Claude Code (in VS Code terminal) | Included in Claude Pro (~$20/mo) |
| Data backend | Google Sheets published as CSV | Free — no API key needed |
| App format | Single HTML file, vanilla JS | Free |
| Maps | Google Maps JavaScript API | Free tier (10K loads/mo), $0 hard cap |
| Offline | PWA service worker | Free |
| Version control | GitHub private repo | Free |
| Hosting | GitHub Pages | Free |
| Expense storage | localStorage (on-device) | Free |

**Total ongoing cost: ~$20/month (Claude Pro only). Everything else is permanently free.**

---

## 7. Google Sheets Setup (Do This Before Coding)

**Step 1 — Create the Sheet**

Create a new Google Sheet called "UK Trip 2026 — App Data" with five tabs: config, itinerary, places_london, places_edinburgh, checklist. Set up column headers matching the names in Section 5. Claude can help generate initial CSV content to paste in.

**Step 2 — Publish Each Tab to Web**

For each tab: File > Share > Publish to web > select the tab > select CSV > Publish > copy the URL.

The URL format is:
https://docs.google.com/spreadsheets/d/SHEET_ID/pub?gid=TAB_ID&single=true&output=csv

Save all five URLs. You will paste them into the app's configuration file when building.

Important: the Sheet must stay published for the app to load fresh data. If you unpublish it by accident, just re-publish.

**Step 3 — Google Maps API Key**

1. Go to console.cloud.google.com, create a free account
2. Create a project called "UK Trip 2026"
3. Enable "Maps JavaScript API"
4. Create an API key and restrict it to your GitHub Pages URL
5. Billing > Budgets and Alerts > create a $0 budget with 100% alert
6. Store the key in a config.js file and add config.js to .gitignore

---

## 8. Build Order (Learning Roadmap)

**Phase 1 — Environment setup (1 evening)**
Install Node.js. Install Claude Code (npm install -g @anthropic-ai/claude-code). Open VS Code. Run a test prompt. Open the result in a browser. Goal: comfort with the terminal and the Claude Code loop.

**Phase 2 — Google Sheet setup and data entry (1 evening)**
Create the Sheet with all five tabs. Populate config and checklist. Start the itinerary with London data. Publish tabs and save CSV URLs. No coding yet — this is the data foundation.

**Phase 3 — Itinerary display (2 evenings)**
Claude Code builds the HTML/CSS/JS that fetches from the itinerary tab and renders day cards. First time seeing real trip data in a real app. Core concept: fetching and displaying data from an external source.

**Phase 4 — Checklist (1 evening)**
Add checklist section reading from the checklist tab, with localStorage for checkbox state. Core concept: saving state locally on the device.

**Phase 5 — Budget tracker (1-2 evenings)**
Add budget section with expense form and localStorage persistence. Core concept: forms, user input, and user-created data.

**Phase 6 — Map integration (1-2 evenings)**
Add Google Maps with place data from the places tabs. Colour-coded pins, filters, city toggle. Core concept: third-party API integration.

**Phase 7 — Connect itinerary to map (1 evening)**
Clicking a day updates the map to show that day's route. Core concept: two sections of an app communicating with each other.

**Phase 8 — Bilingual toggle (1 evening)**
EN/FR toggle for all interface labels. Core concept: state management and internationalisation (i18n).

**Phase 9 — PWA, offline, and deploy (1 evening)**
Service worker for offline caching. Push to GitHub. Enable GitHub Pages. Real URL on your phone. Done.

---

## 9. First Prompt for Claude Code (Use This Verbatim)

Use at the start of Phase 3, after the Sheet is set up and published:

"I'm building a personal travel companion web app for a UK trip in July 2026. The app reads all its data from a Google Sheet published as CSV. No frameworks — just vanilla HTML, CSS, and JavaScript in a single file. Start by building the itinerary section: fetch the itinerary CSV tab on load, parse it, and display each day as a card showing the date, day theme, and list of stops with their category and cost flag. Include a toggle to switch between London days and Edinburgh days. Make it mobile-friendly with a clean minimal design. Here are my CSV URLs: [paste your 5 URLs here]"

---

## 10. Maintenance Workflow (Once Built)

**Change an activity:** Edit the Sheet row. Reload the app. Under 60 seconds.

**Add a map pin:** Add a row to the places tab with name, category, lat/lng. Save.

**Add a checklist item:** Add a row to the checklist tab. Save.

**Update budget amounts:** Update the config tab. Save.

**Update the GBP/CAD exchange rate before the trip:** Update config tab. Save.

**The only time you open VS Code and Claude Code is when you want to change how the app looks or behaves — never just what it shows.**

---

## 11. Key Reminders

- **Book trains now.** Prices increase closer to July. Use lner.co.uk or Trainline.
- **Book Harry Potter Studios now.** Sells out weeks in advance. Transport + entry package at wbstudiotour.co.uk.
- **UK ETA required.** Canadian citizens need an Electronic Travel Authorisation (15 GBP/person). Apply at gov.uk/eta.
- **Airbnb is in Shepherd's Bush.** Plan London days geographically to avoid unnecessary crosstown travel.
- **Edinburgh is under-planned.** Only 5 restaurants saved, no landmarks or activities yet. Plan Edinburgh in the project chat before building that section.
- **Gatwick hotel for July 19.** Bloc Hotel (inside South Terminal) is most convenient. Book soon.
- **Keep the Sheet published.** If you unpublish it accidentally, just re-publish to fix it.

---

Document prepared: March 2026
Version 2.0 — Google Sheets data backend architecture (Option B)
Supersedes Version 1.0 (static data baked into code)
