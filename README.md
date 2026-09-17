# Hyrox Race Calculator & Analyzer

Single-file HTML app (no build, no dependencies, no server) to plan, import, analyse and store Hyrox race results — 8 km of running split by 8 workout stations.

**Live:** https://fmiguelt.github.io/hyrox-race-analyzer

Or open `hyrox_calculator.html` locally in any modern browser — it works the same offline. All data lives in the browser's `localStorage`.

## Features

### 1. Race Planner
Enter a target finish time and an estimated total station time; pick a pacing strategy (even, conservative, aggressive) and get the target time and pace for each of the 8 × 1 km runs.

### 2. Quick Import from Results
Paste splits copied from an official results page. Lines use `R.` for runs and `E.` for exercises/stations, in race order:

```
R. 05:13  R. 05:16  R. 05:37  R. 05:14  R. 05:48  R. 06:11  R. 05:25  R. 04:48  1:18:17  +18:20
E. 04:11  E. 02:01  E. 03:42  E. 03:19  E. 04:59  E. 02:28  E. 05:02  E. 04:21
```

The parser fills all 16 split fields and picks up the standalone `HH:MM:SS` value as the official final time (ignoring `+` gap times).

### 3. Split Analyzer
Manual or imported splits, analysed into:
- total time, total running time and total station time (with percentages)
- **Rox Zone** — transition time, derived as `official final time − sum of all splits` (needs the official time to be filled in)
- average run pace, fastest and slowest 1 km
- a per-segment table with runs, stations and the Rox Zone row

### 4. Save Race Result
Stores the current splits plus race name, partner (shown only for Doubles/Relay categories), location, category, date, official results URL, and a **Competition / Simulation** flag.

### 5. Race History
Saved races sorted newest first, filterable by All / Competition / Simulation. Each entry can be **Loaded** (splits back into the form), **Edited** (update in place) or **Deleted**. `Export Data` writes a JSON backup; `Import Data` merges it back, skipping races whose `id` already exists.

### 6. Performance Reference Guide
Benchmark ranges per level (Elite / Advanced / Intermediate / Beginner) for run pace, total 8 km, each station and the station total, across 7 categories: Men & Women Open, Men & Women Pro, Men & Women Doubles, Mixed Doubles.

## Stations (in race order)

| # | Station | Distance / Reps |
|---|---------|-----------------|
| 1 | SkiErg | 1000 m |
| 2 | Sled Push | 50 m |
| 3 | Sled Pull | 50 m |
| 4 | Burpee Broad Jumps | 80 m |
| 5 | Rowing | 1000 m |
| 6 | Farmers Carry | 200 m |
| 7 | Sandbag Lunges | 100 m |
| 8 | Wall Balls | 100 reps  |

## Time formats

Splits accept `MM:SS`; the final/target time accepts `MM:SS` or `HH:MM:SS`.

## Data & storage

- Key: `hyrox_races` in `localStorage`, an array of race objects (`id`, splits, totals, metadata).
- Nothing is sent anywhere — clearing browser data for the page deletes the history, so use `Export Data` for backups.
- Storage is per-origin: races saved on https://fmiguelt.github.io/hyrox-race-analyzer are not visible to a local copy of the file, and vice-versa. Move them across with `Export Data` / `Import Data`.
- Export filename: `hyrox-data-YYYY-MM-DD.json`.

## Tech notes

- Plain HTML + CSS + vanilla JS in one file, no external assets.
- Responsive: the two-column split inputs and the save fields collapse to one column below 768 px.
- User-supplied strings are escaped before being rendered into history entries.
- The import parser uses a lookbehind regex for the final time, so it needs a browser with lookbehind support (Chrome/Edge 62+, Firefox 78+, Safari 16.4+).
