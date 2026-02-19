# BizGap — Business Opportunity Intelligence

> A single-file, AI-powered web application that helps entrepreneurs and existing business owners discover **untapped market gaps** in localities across **Bhubaneswar, Odisha**.

---

## Table of Contents

1. [Overview](#overview)
2. [Features](#features)
3. [How It Works — User Flow](#how-it-works--user-flow)
4. [Tech Stack & Dependencies](#tech-stack--dependencies)
5. [File Structure](#file-structure)
6. [Location Database](#location-database)
7. [Gap Analysis Engine](#gap-analysis-engine)
8. [Results & Visualizations](#results--visualizations)
9. [Design System & UI](#design-system--ui)
10. [Configuration & API Keys](#configuration--api-keys)
11. [Running the App](#running-the-app)
12. [Known Limitations](#known-limitations)
13. [Future Improvements](#future-improvements)

---

## Overview

**BizGap** is a fully self-contained single HTML file (`index.html`) that simulates a business intelligence dashboard. It guides users through a **3-step wizard** to collect their business profile and target locality, then generates a rich, data-driven analysis of the market gap, competition saturation, demand signals, and business opportunity score — all rendered entirely client-side with no backend required.

The app is specifically tailored for **Bhubaneswar**, with a handcrafted database of 30+ localities containing anchor institutions, demographic profiles, and category-level market biases.

---

## Features

### Core Features
- **3-Step Guided Wizard** — Business status → Category selection → Locality picker
- **Gap Score Gauge** — A circular SVG gauge displaying the opportunity score (0–100)
- **Gap Dimension Bars** — Animated bars for Demand, Competition, Saturation, Growth, and Infrastructure
- **Market Metrics Row** — Key KPIs: Market Attractiveness, Demand Index, Growth Outlook, and Infrastructure Score
- **Opportunity Cards** — AI-generated ranked business opportunities with viability tags, ROI estimates, timeline, risk rating, and detailed breakdowns
- **Anchor Point Intelligence** — Shows nearby institutions (universities, hospitals, IT parks, transit hubs) that drive demand for each locality
- **Locality Profile Panel** — Population, income bracket, resident type, construction activity, and footfall rating
- **Two-Column Summary Panels** — Location Personality + Market Intelligence presented side-by-side
- **Interactive Heatmap** — Uses Google Maps (with canvas fallback) to visualize competition zones, gap zones, and anchor points
- **Toast Notifications** — Lightweight feedback toasts for user interactions
- **Reset Flow** — Full state reset to start a new analysis without page reload

### UX & Design Features
- Animated background canvas with drifting radial gradient orbs
- Step progress indicator with active/completed state transitions
- Smooth page transitions (fade + translate) between wizard steps
- Glassmorphism card design throughout
- Fully responsive for mobile screens
- Animated counters on the hero page (`data-t` attribute)

---

## How It Works — User Flow

### Step 1 — Business Status
User selects one of two modes:
- **Starting Fresh** — Looking to identify a new business opportunity
- **Already Running** — Looking to find growth or expansion opportunities for an existing business

The selection adapts the copy in Step 2 to match the user context.

### Step 2 — Category Selection
User selects a **sector** from a 4-column icon grid:

| Sector | Sub-options (examples) |
|---|---|
| 🍽️ Food & Beverage | Cloud Kitchen, Bakery, Cafe, Restaurant, Food Truck |
| 🛍️ Retail | Electronics, Clothing, Grocery, Home Decor |
| 🏥 Healthcare | Clinic, Pharmacy, Diagnostic Lab, Dental Clinic |
| 📚 Education | Coaching Institute, Preschool, Computer Classes |
| 💻 Tech & IT | IT Services, Web Agency, Mobile Repair |
| 🔧 Services | Laundry, Car Wash, Salon, Pest Control |
| 🏨 Hospitality | Budget Hotel, Guest House, Co-working Space |
| 💄 Beauty & Wellness | Spa, Nail Studio, Yoga Center |
| 🚚 Logistics | Courier, Packers & Movers, Warehouse |
| 💰 Finance | Loan Agency, Insurance, CA Services |
| 🏠 Real Estate | Broker, Interior Design, Property Management |
| 🌾 Other | Agriculture, Printing, Handicrafts, Pet Services |

After selecting a category, optional **sub-category pills** appear to narrow the focus.

### Step 3 — Locality Selection
User picks a locality from a dropdown of **30 Bhubaneswar localities**. On selection:
- A **mini Google Map** renders with a purple marker on the selected locality
- The "Analyse" button activates

### Loading Screen
A 4-step animated loading sequence plays, simulating:
1. Scanning anchor points
2. Mapping demand signals
3. Calculating gap score
4. Building opportunity report

### Results Page
A scrollable, full-width results page renders with all visualizations described in the [Results & Visualizations](#results--visualizations) section.

---

## Tech Stack & Dependencies

| Dependency | Source | Purpose |
|---|---|---|
| **Google Maps JavaScript API** | CDN (`maps.googleapis.com`) | Interactive mini-map and results heatmap |
| **Google Maps Places & Geometry Libraries** | Loaded with Maps API | Geocoding support (not directly used for display) |
| **Google Maps Visualization Library** | Loaded with Maps API | `HeatmapLayer` for the results heatmap |
| **DM Serif Display** | Google Fonts | Display/heading typeface |
| **DM Sans** | Google Fonts | UI body/label typeface |

**No frameworks. No build tools. No backend.** Everything runs in vanilla HTML, CSS, and JavaScript.

---

## File Structure

The entire application is a single file:

```
index.html
├── <head>
│   ├── Meta tags & viewport
│   ├── Google Fonts links
│   ├── Google Maps API script
│   └── <style> — Full CSS (design system, layout, components)
│
└── <body>
    ├── <canvas id="bgc"> — Animated background
    │
    ├── Pages (each is a .page div, shown/hidden via JS router)
    │   ├── #pg-hero   — Landing/hero page with stats
    │   ├── #pg-s1     — Step 1: Business status
    │   ├── #pg-s2     — Step 2: Category selection
    │   ├── #pg-s3     — Step 3: Locality selection + mini map
    │   ├── #pg-load   — Loading animation screen
    │   └── #pg-res    — Results page (all cards, heatmap, charts)
    │
    ├── #toast — Toast notification element
    │
    └── <script>
        ├── Background canvas animation
        ├── Counter animation (data-t)
        ├── Page router (go function)
        ├── State object (S)
        ├── Step 1–3 handlers (selStatus, selCat, selSub, onLoc)
        ├── Mini map renderer (drawMiniMap — Google Maps or canvas)
        ├── LOC_DB — Location database (30 localities)
        ├── DEFAULT_PROFILE — Fallback profile for unlisted localities
        ├── getExtraAnchors / getAnchors — Anchor resolution
        ├── getLocProfile — Locality demographic profile
        ├── getGapProfile — Gap analysis scoring engine
        ├── generateOpportunities — Opportunity card generator
        ├── buildResults — Full results DOM builder
        ├── initGoogleHeatmap / drawCanvasHeatmap — Heatmap renderers
        ├── seeded — Seeded pseudo-random number generator
        ├── reset — Full state reset
        └── toast — Notification helper
```

---

## Location Database

The `LOC_DB` object contains detailed profiles for **30 Bhubaneswar localities**:

```
Patia, Chandrasekharpur, Jaydev Vihar, Damana, Infocity,
KIIT Area, KISS Area, Niladri Vihar, Dumduma, Aiginia,
Tamando, Nayapalli, IRC Village, Saheed Nagar, Kharavela Nagar,
Acharya Vihar, Vani Vihar, Rasulgarh, Mancheswar, Kalinga Nagar,
Bomikhal, Pokhariput, Baramunda, Khandagiri, Airport Area,
Railway Station Area, AIIMS Bhubaneswar, SUM Hospital Area,
Old Town, Lingaraj Area
```

### Each Locality Entry Contains:

**`anchors`** — Array of 4–5 key institutions with:
- `n` — Full name
- `t` — Type (e.g., "University", "IT Hub", "Transit Hub")
- `i` — Emoji icon

**`profile`** — Demographic snapshot:
- `pop` — Approximate population
- `growth` — Growth narrative
- `income` — Income bracket (Mid / Upper-middle / Affluent)
- `type` — Resident persona (e.g., "IT Professionals", "Students + Faculty")
- `newConstruction` — Boolean, whether the area has active construction
- `footfall` — Footfall rating string

**`catBias`** — Per-category scoring object with 5 dimensions (0–100):
- `demand` — Market demand intensity
- `competition` — Existing competitor density
- `saturation` — Market saturation level
- `growth` — Growth trajectory
- `infra` — Infrastructure readiness

Categories covered per locality: `food`, `retail`, `healthcare`, `education`, `tech`, `services`, `hospitality`, `beauty`, `logistics`, `finance`, `real_estate`, `other`.

---

## Gap Analysis Engine

The `getGapProfile(loc, cat, anchors)` function computes all scores.

### Score Resolution Priority:
1. **If** the locality exists in `LOC_DB` **and** has a `catBias` entry for the selected category → use exact database values.
2. **If** not found → use an intelligent anchor-signal fallback:
   - Detects anchor types (university, hospital, IT, transit, industrial, tourist, government, commercial)
   - Applies additive adjustments to base scores (e.g., `hasIT` → demand +18, growth +16)

### Jitter for Uniqueness:
A `jitter()` function applies a small deterministic offset (derived from the locality name's character code sum) so no two localities produce identical numbers.

### Derived Scores:
```
gapScore   = 100 − (saturation×0.38 + competition×0.32 − demand×0.22)
             → clamped to [34, 93]

marketScore = (demand×0.34 + growth×0.34 + infra×0.20 + (100−competition)×0.12) ÷ 10
             → capped at 9.8
```

### Opportunity Generation (`generateOpportunities`):
Produces 4–6 ranked opportunity cards based on the gap profile. Each card includes:
- Business type name and emoji
- Viability tag (e.g., "High Viability", "Strong Fit")
- Match percentage score
- ROI estimate (e.g., "18–26 months")
- Risk level (Low / Medium / High)
- 3-point insight breakdown (Demand signal, Gap reason, Positioning recommendation)
- Expandable "Full Analysis" section with target audience, competitive edge, and 3 action steps

---

## Results & Visualizations

### 1. Business Opportunity Gap Hero Card
The primary result card shows:
- Gap Score gauge (SVG arc, color-coded: green ≥ 70, amber 50–69, red < 50)
- Gap score title and narrative description
- 5 animated dimension bars (Demand, Competition, Saturation, Growth, Infrastructure)
- 4 metric tiles (Market Attractiveness, Demand Index, Growth Outlook, Infrastructure Score)

### 2. Opportunity Cards (`#opp-section`)
Ranked list of 4–6 business opportunities with expandable detail drawers.

### 3. Anchor Points Panel
Lists 4–5 nearby institutions with type and emoji, explaining why they drive demand.

### 4. Locality Profile Panel
Displays population, income, resident type, footfall, and growth tag (with a pulsing "New Construction" badge if applicable).

### 5. Two-Column Grid (`#two-col-grid`)
Side-by-side panels for:
- **Location Personality** — Narrative description of the locality's character
- **Market Intelligence** — Category-specific market insights

### 6. Opportunity Heatmap
Rendered at the bottom of the results page. Uses either:
- **Google Maps HeatmapLayer** — If the Maps API loads successfully (renders a real-world map with a `HeatmapLayer` of demand-weighted points, blue anchor markers, and a purple "you are here" marker)
- **Canvas Fallback** — If Maps API is unavailable; draws a stylized abstract heatmap with color-coded zones:
  - 🔴 Red — High Competition Zones
  - 🟠 Amber — Partial Gap / Moderate Competition
  - 🟢 Green — Clear Business Voids (opportunity spots)
  - 🔵 Blue — Anchor Points
  - 🟣 Purple — Your Selected Location

---

## Design System & UI

### CSS Custom Properties (`:root`)

| Variable | Value | Use |
|---|---|---|
| `--bg` | `#04080f` | Page background |
| `--accent` | `#4f8aff` | Primary blue accent |
| `--accent2` | `#9b5cff` | Purple secondary accent |
| `--gold` | `#d9a84e` | Italic/highlight gold |
| `--green` | `#18c49a` | Positive/gap indicators |
| `--red` | `#f05252` | High competition/risk |
| `--amber` | `#e8943a` | Moderate signals |
| `--text` | `#edf0f8` | Primary text |
| `--muted` | `rgba(237,240,248,.44)` | Secondary/muted text |
| `--display` | DM Serif Display | Heading font |
| `--ui` | DM Sans | Body/UI font |

### Key Component Classes

| Class | Description |
|---|---|
| `.page` | Full-screen page layer; `.visible` shows it, `.exit` fades it out |
| `.card` | Glassmorphism card with blur + border |
| `.ch` | Choice card (Step 1) with hover/selected states |
| `.cat` | Category grid button (Step 2) |
| `.sub` | Sub-category pill button |
| `.btn-p` | Primary gradient button |
| `.btn-g` | Ghost/secondary button |
| `.opp-card` | Opportunity result card |
| `.rc` | Result section card |
| `.prog` | Step progress bar container |
| `.pd` | Progress dot (`.act` = active, `.dn` = done) |

---

## Configuration & API Keys

### Google Maps API Key
The Maps API key is hardcoded in the script tag:

```html
<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&libraries=places,geometry,visualization" async defer></script>
```

> ⚠️ **Important:** The key currently in the file is exposed publicly. Before deploying, replace it with a properly **restricted key** (restricted to your domain in the Google Cloud Console). If the Maps API fails to load, the app automatically falls back to the canvas-based heatmap — no functionality is broken.

---

## Running the App

Since BizGap is a single static HTML file with no build step:

1. **Clone or download** `index.html`
2. **Open it directly** in any modern browser:
   ```
   open index.html
   ```
   Or serve it via any static file server:
   ```bash
   npx serve .
   # or
   python3 -m http.server 8080
   ```
3. No `npm install`, no environment setup, no backend needed.

> **Browser Support:** Works in all modern browsers (Chrome, Firefox, Edge, Safari). Requires JavaScript to be enabled.

---

## Known Limitations

- **Bhubaneswar only** — The location dropdown and `LOC_DB` are specific to Bhubaneswar localities. The app is not designed for other cities out of the box.
- **Simulated data** — All scores and opportunity insights are algorithmically generated from a static database. No live market data API is used.
- **No persistence** — There is no save, export, or share functionality. Closing the tab loses all results.
- **API key exposure** — The Google Maps API key is hardcoded and visible in source. Restrict it before public deployment.
- **Canvas heatmap is abstract** — The fallback canvas heatmap is decorative/illustrative and does not reflect real geographic coordinates.
- **No authentication** — The app has no login, user accounts, or access control.

---

## Future Improvements

- Add more cities beyond Bhubaneswar
- Integrate a live data source (Google Places API, census data) for real competition density
- Add a PDF/PNG export for the results report
- Implement a share link with encoded state in the URL
- Add comparison mode (compare two localities side by side)
- Allow users to weight the gap dimensions (e.g., prioritize growth over infrastructure)
- Persist analysis history in `localStorage`
- Add a "Similar Localities" recommendation panel

---

*Built as a single-file static web app. No framework, no build step, no backend.*
