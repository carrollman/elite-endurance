# Paul Carroll — Elite Endurance Performance Cockpit

**Version:** v10  
**Season:** March – September 2026  
**Events:** Killarney 10k (9 May) · Ring of Kerry 170km (4 Jul) · Dingle Half Marathon (5 Sep)

---

## What This Is

A self-contained mobile-first performance dashboard for a hybrid endurance athlete combining running, cycling, and strength training across a 25-week season. The dashboard operates as a decision-support tool — not just a training diary. It tells you what is trending, what is at risk, and what to do next.

It runs entirely in the browser as a single HTML file with no server, no login, and no dependencies beyond an internet connection for the custom fonts. All 175 planned sessions are embedded directly in the file, so it works offline immediately. When connected to a Google Sheet via Apps Script, it reads live logged data on top of the embedded plan.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | The dashboard — rename and deploy as-is |
| `google_apps_script.js` | Paste into Google Apps Script to connect your Sheet |
| `.gitlab-ci.yml` | GitLab Pages deployment pipeline |
| `README.md` | This file |

---

## Quick Start

### Option A — Open locally
Download `index.html` and open it in any browser. The embedded data loads immediately. Navigate using the bottom tab bar.

### Option B — Host on GitLab Pages (recommended)
1. Create a GitLab repository
2. Upload `index.html` (keep the filename exactly) and `.gitlab-ci.yml`
3. Push to `main` — the pipeline runs automatically (~60 seconds)
4. Access at `https://yourusername.gitlab.io/your-repo-name`
5. Connect your Google Sheet (see Setup section below)

---

## Navigation

Six tabs across the bottom of the screen:

| Tab | Icon | What it shows |
|-----|------|---------------|
| **Command** | ⚡ | Today's session, coaching directive, intelligence alerts, race readiness scores, season KPIs, stacked load chart, heatmap |
| **Load** | 📊 | ACWR gauge, monotony, strain, discipline load bars, zone distribution, RPE vs fatigue chart |
| **Quality** | 🎯 | Key session hit rate, threshold count, long run/ride progression, high-value session table |
| **Plan** | 🗓 | Full 25-week plan with phase, goal, and coaching focus per week |
| **Races** | 🏁 | Per-race pacing strategy, fuelling protocol, race week schedule, readiness score, Riegel predictor |
| **Setup** | ⚙️ | Google Sheets connection URL, test button, logging instructions |

---

## The 25-Week Plan

The plan is structured around three race targets with distinct training blocks:

| Weeks | Phase | Priority |
|-------|-------|----------|
| W1–2 | Foundation + 10k prep | Re-establish frequency, rhythm, and strength base |
| W3 | 10k build | Threshold density |
| W4 | Deload | Absorb block, freshen |
| W5–6 | 10k specific | Race pace efficiency, threshold + economy |
| W7 | 10k taper | Sharpen and freshen |
| W8 | 10k race week | Killarney 10k — 9 May |
| W9 | Transition + bike base | Shift emphasis to bike durability |
| W10–12 | Bike build | Aerobic bike volume, muscular endurance, long-ride durability |
| W13–14 | Ring build / peak | Tempo under fatigue, big endurance weekend |
| W15 | Ring taper / race | Ring of Kerry 170km — 4 Jul |
| W16 | Run rebuild | Restore run rhythm post-Ring |
| W17–20 | Half build | Threshold density, race pace durability, long-run progression |
| W21–22 | Half specific | Sustained threshold, peak specificity |
| W23 | Half taper | Drop volume, keep touch |
| W24 | Race week | Dingle Half Marathon — 5 Sep |
| W25 | Recovery | Unload and reset |

### Key Sessions
Sessions marked ★ in the plan are key sessions — threshold runs, intervals, long runs, long rides, and race days. These are tracked separately from general sessions because missing a key session costs more fitness than missing an easy run. The dashboard shows key session hit rate independently of overall completion rate.

### Deload Weeks
Week 4 is a planned deload. Volume drops and intensity is capped to allow the body to absorb the preceding block. Do not add extra sessions during deload weeks.

---

## Performance Metrics Explained

### sRPE Load
**Session RPE Load** is the primary training load unit throughout the dashboard.

```
sRPE Load = Duration (minutes) × RPE (1–10)
```

A 55-minute run at RPE 7 = load of 385. This is calculated automatically in the Google Sheet — you only need to fill in Duration and RPE.

The RPE scale used is the full 1–10 Borg CR10 scale applied to the **whole session** (not just the hardest part), judged approximately 30 minutes after finishing:

| RPE | Description |
|-----|-------------|
| 1–2 | Very easy — walk, mobility, barely moving |
| 3–4 | Easy — full conversation, Z1–Z2 |
| 5–6 | Moderate — short sentences, Z2–Z3 |
| 7–8 | Hard — intervals, tempo, quality work, Z4 |
| 9 | Very hard — race effort |
| 10 | Maximal — unsustainable |

### ACWR (Acute:Chronic Workload Ratio)
Compares this week's load (acute) against the rolling 4-week average (chronic). The dashboard calculates this automatically from logged sRPE loads.

| ACWR | Status |
|------|--------|
| Below 0.8 | Under-loading — fitness may be detraining |
| 0.8 – 1.3 | **Optimal zone** — safe to build |
| 1.3 – 1.5 | Caution — load has jumped, monitor fatigue |
| Above 1.5 | **Reduce load** — elevated injury risk |

### Monotony
Measures how varied your training is across the rolling 4-week window. Calculated as mean weekly load divided by the standard deviation. Values above 2.0 flag that training has become too repetitive — variety between hard and easy days is insufficient.

### Strain
Strain = Total Load × Monotony. A composite that combines volume and variety risk into one number. High strain with high monotony is the warning combination.

### Readiness Score
A composite score out of 10 calculated from:
- **Fatigue (1–5)** — higher fatigue reduces readiness
- **Sleep (hours)** — below 7 hours reduces readiness

The formula: `10 − (fatigue penalty) − (sleep penalty)`. Only calculated for weeks where you have entered fatigue and/or sleep data. Visible on the Command tab and feeds the race readiness models.

### Race Readiness Score
Each race has its own readiness model, shown on both the Command and Races tabs:

- **Killarney 10k** — weighted from key session hit rate (60%) and longest run relative to 14km target (40%)
- **Ring of Kerry** — weighted from longest ride relative to 150km target (60%) and average weekly readiness (40%)
- **Dingle Half Marathon** — weighted from half-specific key session hit rate (60%) and longest run relative to 18km target (40%)

Scores are displayed out of 10. Below 6 is Early/Building. 6–8 is Building/On Track. Above 8 is On Track.

---

## Logging Sessions

### Where to log
Open your Google Sheet → `Training_Log` tab. Each session date is pre-populated for all 175 sessions across 25 weeks. Find today's row and fill in the columns after completing the session.

### What to fill in

| Column | What to enter | Required? |
|--------|--------------|-----------|
| `Completed?` | Yes / No / Modified | ✓ |
| `Completion Quality` | Hit / Modified / Partial / Missed | ✓ |
| `Actual Description` | What you actually did — keep it brief | ✓ |
| `Run Distance (km)` | Total including warm-up/cool-down | If run |
| `Cycle Distance (km)` | Total distance | If ride |
| `Duration (min)` | Total session time | ✓ |
| `RPE` | 1–10 whole session RPE | ✓ |
| `sRPE Load` | **Leave blank** — formula auto-calculates | — |
| `Fatigue (1-5)` | How fatigued you feel after the session | Recommended |
| `Sleep (h)` | Last night's sleep in hours | Recommended |
| `Intensity Zone` | Z1 / Z2 / Z3 / Z4 / Z5 | Optional |
| `Notes` | Anything worth recording | Optional |

### Completion Quality definitions
- **Hit** — session was completed as planned
- **Modified** — completed but with changes (shorter, easier, different session)
- **Partial** — started but did not finish
- **Missed** — session not completed

### What to leave blank
The following columns are either pre-filled or auto-calculated — do not overwrite them:
- `Date`, `Day`, `Week`, `Phase` — pre-populated
- `Planned Session`, `Discipline`, `Session Type`, `Session Subtype`, `Key Session?`, `Session Goal` — pre-populated from the plan
- `sRPE Load`, `Strength Load` — formula-driven

---

## Strength Program

Strength prescription changes across three blocks to manage interference with endurance load:

| Block | When | Sessions | Main Lift | Dose |
|-------|------|----------|-----------|------|
| 10k build | W1–8 | A (Mon) + B (Thu) | Back Squat / Deadlift | Moderate — 3–4 sets, 4–6 reps |
| Bike build | W9–15 | A (Mon) + B (Thu) | Trap Bar Deadlift / Goblet Squat | Low-moderate — protect ride freshness |
| Half build | W16–22 | A (Mon) + B (Thu) | Squat 2–3×4 / Single-leg pattern | Low — maintenance and tissue robustness |
| Taper weeks | W7, W15, W23–25 | One light session | 2×3 at ~80% | Low — neural activation only |

### Interference rules
- Allow 24–48 hours between lower-body strength and quality run sessions
- No heavy lower-body strength within 24 hours of a long ride
- Stop all strength 7 days before each A-race
- If DOMS affects running gait, skip the strength session — run quality always wins

---

## Google Sheets Connection

The dashboard works offline using embedded data. To use live logged data from your Google Sheet:

1. Open your Google Sheet → **Extensions → Apps Script**
2. Delete any existing code and paste the full contents of `google_apps_script.js`
3. Click **Run → testScript** and verify the log shows correct row counts
4. Click **Deploy → New deployment**
   - Type: **Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
5. Copy the URL ending in `/exec`
6. Open the dashboard → **Setup tab** → paste the URL → tap **Save & Connect**

Once connected, the dashboard fetches live data on every load and shows a green **Live** badge in the header. If the connection fails it falls back to the embedded data silently.

### What the Apps Script returns
The script reads three sheets and returns them as JSON:
- `data` — Training_Log (primary — drives all charts and calculations)
- `plan` — Weekly_Plan
- `readiness` — Readiness_Log

---

## Technical Notes

### Architecture
- Single HTML file, no build step, no framework
- Vanilla ES5 JavaScript throughout (no arrow functions, no destructuring, no template literals) — compatible with all browsers
- Chart.js 4.4.1 loaded from CDN for charts
- Barlow Condensed + Barlow fonts from Google Fonts
- All training data embedded as JavaScript arrays at the top of the script block
- `localStorage` used only to store the Apps Script URL between sessions

### Data flow
```
Google Sheet (Training_Log)
       ↓
Apps Script Web App (/exec?action=getData)
       ↓
fetchData() → LIVE[] array
       ↓
calcWeeks() → weekly stats with ACWR, monotony, readiness
       ↓
cmdPage / loadPage / qualityPage → rendered HTML + Chart.js
```

If no URL is configured, all pages use the embedded `LOG_DATA` array instead of `LIVE`.

### Updating the embedded data
When the workbook is updated (new sessions logged, plan changed), regenerate the embedded data by running the Python extraction script and rebuilding the HTML. The embedded data serves as a baseline and backup — it does not need to be kept perfectly current if live Sheets sync is active.

### GitLab Pages deployment
The `.gitlab-ci.yml` pipeline:
1. Creates a `public/` directory
2. Copies `index.html` into it
3. Deploys it as a GitLab Pages site

The pipeline runs automatically on every push to `main`. Update the dashboard by pushing a new `index.html` — the site updates within ~60 seconds.

---

## Targets

| Race | Date | Target | Predicted (Riegel) |
|------|------|--------|-------------------|
| Killarney 10k | 9 May 2026 | Sub 46:00 | 47:50 (from 5k 23:00) |
| Ring of Kerry | 4 Jul 2026 | Strong finish | ~6–7 hrs |
| Dingle Half Marathon | 5 Sep 2026 | Sub 1:40:00 | 1:42:19 (from 10k PB 48:16) |

Riegel formula: `T₂ = T₁ × (D₂/D₁)^1.06`

Update the Race_Predictor sheet in the workbook as 5k and 10k times improve. The Races tab in the dashboard will reflect updated predictions on next sync.
