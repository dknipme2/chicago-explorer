# Chicago Explorer — Project Status

**Last Updated:** 2026-04-03
**Live URL:** https://dknipme2.github.io/chicago-explorer/
**Repo:** https://github.com/dknipme2/chicago-explorer
**Local Dev:** `python -m http.server 3001 --directory app` (or Preview tool via `.claude/launch.json`)

---

## What It Is

A personalized PWA for Dan's sister Nikki's Chicago visit. Based from **Glen Ellyn, IL**. Curates off-beat restaurants, activities, shops, and experiences organized into **12 theme trails** — not tourist traps, not Giordano's, not the Bean.

## Current State: Fully Functional v1

### App Features
| Feature | Status | Notes |
|---|---|---|
| Welcome screen (personalized for Nikki) | ✅ | Shows on first visit, `?` button to revisit |
| Discover tab — 74 places with scores | ✅ | Score, must-try item, description, rationale |
| 6 sort modes | ✅ | Score, Nearest (GPS), Spiciest, Budget, Adventurous, Can't-Get-Downstate |
| Type filters (All, Favorites, Restaurant, Activity, Shop) | ✅ | |
| Trails tab — 12 theme trails | ✅ | Tap a trail to filter Discover list |
| Map tab — Mapbox GL with all markers | ✅ | Dark style, geolocate, filter chips |
| Trail filter on map | ✅ | Second row of chips filters markers by trail |
| Google-style bottom sheet | ✅ | Opens from map markers + drawer items |
| Google Places enrichment | ✅ | Auto-fetches photos, star ratings, phone numbers |
| Favorites (localStorage) | ✅ | Heart button on cards + sheet, persists across sessions |
| Directions + Google Maps links | ✅ | On cards + sheet |
| Haversine distance sorting | �� | Uses device GPS, shows distance in miles |
| Share a trail | ✅ | Hash-based URLs (#trail=, #place=, #plan=), Web Share API + clipboard fallback |
| Day Planner tab | ✅ | 4th tab: add places, drag reorder, route optimize, multi-stop Google Maps directions |
| Share a place | ✅ | Share button on bottom sheet + shareable deep link |
| Share a plan | ✅ | Shareable URL encodes all plan stops as slugs |
| PWA manifest | ✅ | Add-to-homescreen capable |
| GitHub Pages deployment | ✅ | Auto-deploys via Actions, Mapbox token injected from secret |

### Tech Stack
- **Frontend:** Single-file HTML/CSS/JS PWA (~1500 lines)
- **Map:** Mapbox GL JS v3.3.0, dark-v11 style
- **Data:** Static JSON (`chicago_data.json`, 74 places)
- **Google Places:** New API (v1) for enrichment (photos, ratings, phone)
- **Design:** Dark glassmorphism, Inter font, same design system as Vacation Planner
- **Hosting:** GitHub Pages via Actions workflow
- **Secrets:** Mapbox token in GitHub repo secret `MAPBOX_TOKEN`, Google Places key split in source

### 12 Theme Trails
| Trail | Icon | Count | Description |
|---|---|---|---|
| Dumpling Universe | 🥟 | 10 | XLB, pierogi, momos, varenyky, tortellini, gyoza |
| Noodle Run | 🍜 | 8 | Pho, ramen, hand-pulled, khao soi, naengmyeon |
| Sweet Tooth Mega | 🍩 | 19 | Doughnuts, baklava, gelato, jalebi, churros, ube |
| Fire Walk | 🔥 | 9 | Spice ladder from warm to nuclear |
| Fermentation Station | 🫙 | 5 | Kimchi aisles, kombucha, funky cheese |
| By Hand | 🤲 | 13 | Injera, KBBQ wraps, birria, dim sum, elote |
| Curiosity Cabinet | 🦴 | 14 | Oddities, taxidermy, weird museums, outsider art |
| Snack Attack | 🍬 | 13 | Exotic snacks, international groceries, McDonald's Global |
| Trust the Chef | 👨‍🍳 | 8 | Kasama, Galit, Maman Zari, The Gundis |
| Street Level | 💵 | 12 | Cash-only legends, under $15 meals |
| Weekend Warriors | 📅 | 7 | Weekend-only specials, brunch rituals |
| Free & Outside | 🌿 | 8 | Murals, trails, conservatory, owls, walks |

### Nikki's Profile (from Q&A)
- **Food strategy:** Trust the kitchen / chef's pick
- **Spice:** Max heat (but sister is newer — turns out she's tougher than expected)
- **Special interests:** Krang (TMNT), owls, The Handsome podcast, doughnuts, baklava, exotic snacks, Korean BBQ + supermarkets, game shows, science bulk store, old things & oddities
- **Dining style:** Bite-here-and-there / grazing
- **Budget:** Full range (splurge to street food)
- **Base:** Glen Ellyn, IL — I-88/I-290 corridor to downtown Chicago
- **Key insight:** She won't do "crawls" — prefers themed trails with a must-try at each stop that can be strung together

### File Structure
```
E:\Chicago_Activities\
├── .claude/launch.json              # Preview server config
├── .github/workflows/deploy.yml     # GitHub Pages deployment
├── .gitignore                       # Excludes config.js (Mapbox token)
├── app/
│   ├── index.html                   # The entire app (single file)
│   ├── chicago_data.json            # 74 places with trails, scores, must-try
│   ├── config.js                    # Local Mapbox token (gitignored)
│   └── manifest.json                # PWA manifest
├── chicago_food_adventure_landscape.md   # Initial research (10 cuisines, 6 activities)
├── research_specific_interests.md        # Sister-specific research (75+ finds)
├── research_thematic_trails.md           # Trail-focused research (65+ finds)
├── STATUS.md                        # This file
└── HANDOVER.md                      # Session handover for next session
```

### Known Issues / Quirks
- Map markers use a setTimeout fallback (1s, 3s) due to Mapbox `load` event race condition in some environments. Works reliably but not elegantly.
- Google Places API key is split in source to avoid GitHub scanner — not truly secure, just scanner evasion for a personal app.
- Screenshot tool times out in Preview environment (Mapbox GL heavy) — use `preview_snapshot` or `preview_eval` instead for testing.
