# Chicago Explorer — Session Handover

**Session Date:** 2026-04-03
**Session Duration:** Full build from scratch to deployed v1
**Live:** https://dknipme2.github.io/chicago-explorer/

---

## What Happened This Session

### Phase 1: Research & Architecture
1. Explored **Vacation_Planning** app at `E:\Vacation_Planning` — extracted the design system (dark glassmorphism, Mapbox GL, haversine sorting, card patterns, bottom sheet with Google Places enrichment). This is the reference app.
2. Explored **Commercial_Analytics** at `E:\Commercial_Analytics` — extracted the 6-agent CAP architecture, three-layer knowledge model, and researcher/analyst patterns for how to structure data gathering and scoring.
3. Ran comprehensive web research on Chicago's off-beat food scene: 10 cuisine categories, 6 activity categories, Bib Gourmand, Eater, food blogs.

### Phase 2: Taste Profile Q&A
4. Conducted structured Q&A to build Nikki's preference profile:
   - Food: Trust the kitchen, max spice, sister tougher than expected
   - Activities: Neighborhood walks, art, offbeat museums, outdoor, markets (NO drinking/nightlife)
   - Budget: Full range, flexible timing
5. Second round of Q&A after learning her specific interests (Krang, owls, doughnuts, baklava, exotic snacks, Korean supermarkets, game shows, science store, oddities).

### Phase 3: Theme Trail Concept
6. **Key insight from Dan:** She won't do geographic crawls. Instead, build **theme trails** that cut across cuisines and neighborhoods — "if I say doughnuts, show me all the doughnut spots with a must-try at each."
7. Researched 6 additional thematic trails (Dumpling Universe, Noodle Run, Sweet Tooth, Fermentation Station, By Hand, Weekend Warriors) with 65+ additional spots.
8. Dan approved ALL 12 proposed trails.

### Phase 4: App Build
9. Built the full app as a single-file PWA using the Vacation Planner's design system.
10. Created `chicago_data.json` with 74 places, each tagged with trail memberships, scores, must-try items, and full metadata.
11. Three tabs: Discover (list), Trails (trail picker), Map (Mapbox).
12. Welcome screen personalized for Nikki with `?` button to revisit.

### Phase 5: Features & Polish
13. Added Google-style bottom sheet with Google Places API enrichment (photos, star ratings, phone numbers).
14. Added trail filter chips on map view.
15. Fixed map marker rendering race condition.
16. Added localStorage favorites (heart button on cards + sheet, filter by favorites).
17. Deployed to GitHub Pages with Mapbox token injected from repo secret.

---

## What To Do Next

### High-Value Additions
- **More places:** Currently 74, target was ~100. Research gaps:
  - More Fermentation Station spots (only 5)
  - More Weekend Warriors (only 7)
  - DuPage County suburban spots along the corridor (Elmhurst, Oak Park, Villa Park donut shops are researched but not all in the data file)
  - Specific Krang/TMNT collectible shops
  - Owl-themed items beyond Willowbrook
- **"On the route" badge:** Data has `near-glen-ellyn` and `on-the-route` tags — could add a visual indicator for corridor spots
- **Visited toggle:** Let Nikki mark places as visited (separate from favorites)
- **Notes field:** Let her add personal notes to places
- **Share a trail:** Generate a shareable link for a specific trail
- **Day planner / itinerary builder:** Drag favorites into a day plan with route optimization

### Technical Improvements
- **Service worker:** Add offline caching for the data file and app shell
- **Map marker clustering:** At zoom-out levels, 74 markers can overlap — add clustering
- **Map route lines:** When viewing a trail on map, draw a suggested route between the stops
- **Photo caching:** Google Places photos are fetched fresh each time the sheet opens — could cache in sessionStorage
- **PWA icon:** Currently no app icon — add a custom emoji-based icon

### Research Gaps from Q&A
- **The Handsome podcast:** Nikki likes it — could research if any Chicago spots connect (guests, featured locations)
- **International McDonald's specifics:** Research what's currently on the Global Menu rotation
- **Game Show Battle Rooms:** Confirm current schedule and booking (Lombard location, 5 min from Glen Ellyn)
- **More trail ideas Dan mentioned wanting:** He said "after this round do more trails" — potential new trails:
  - "Brunch Club" — best weekend brunch spots
  - "BYOB Trail" — BYO restaurants (many in Chicago)
  - "Under $10" — ultra-cheap eats
  - "Photo Ops" — most visually stunning spots
  - "Rainy Day" — indoor activities for bad weather days

---

## Key Files to Read First
1. `app/index.html` — The entire app (read the JS section starting around line 530)
2. `app/chicago_data.json` — All 74 places with full metadata and trail tags
3. `research_specific_interests.md` — 75+ researched spots (many not yet in the data file)
4. `research_thematic_trails.md` — 65+ trail-specific research (some not yet in data file)

## Reference App
- `E:\Vacation_Planning\app\index.html` — The Vacation Planner app this was based on. Same design system, same Mapbox token, same Google Places API key.

## Deployment
- Push to `master` triggers GitHub Actions → deploys `app/` folder to Pages
- Mapbox token injected from `MAPBOX_TOKEN` repo secret during build
- Google Places API key is inline (split string to avoid scanners)

---

## Session 2: Share Trail + Day Planner (2026-04-03)

### What Was Added

**Share Trail (hash-based deep linking)**
- URL hash routing: `#trail=dumpling-universe`, `#place=kasama`, `#plan=slug1,slug2,slug3`
- `slugify()` converts place names to URL-safe slugs
- `handleHash()` parses hash on load + `hashchange` events
- Deep links skip the welcome screen automatically
- Web Share API with clipboard fallback (`shareLink()`)
- Share buttons on: trail cards (↗), trail header bar, bottom sheet, and plan
- Toast notification system (`showToast()`) for feedback

**Day Planner tab (4th tab)**
- `planItems[]` persisted in localStorage key `chicago_plan`
- Plan picker: searchable bottom sheet with filter chips (All/Favorites/Restaurant/Activity)
- Drag-to-reorder on desktop (HTML5 Drag API), up/down arrow buttons on mobile (`@media (hover:none) and (pointer:coarse)`)
- Route optimization: nearest-neighbor TSP greedy algorithm using `haversine()`, starts from `userLatLng` or Glen Ellyn (41.8778, -88.0678)
- Stats bar: stop count, total miles, estimated driving minutes (~25mph city average)
- Google Maps multi-stop directions URL with origin/destination/waypoints
- "Add to Plan" button on card actions and bottom sheet
- Share plan via `#plan=slug1,slug2,...` hash URL

**Data updates**
- Enhanced Demera Ethiopian with sensory-experience framing, coffee ceremony, added `trust-the-chef` trail
- McDonald's Global Menu and Game Show Battle Rooms were already in data (added in session 1)
- Total: 74 places

### New localStorage Keys
- `chicago_plan` — JSON array of place name strings (day planner)
- `chicago_favs` — (existing, unchanged)
- `chicago_welcomed` — (existing, unchanged)

### Research Findings
- **The Handsome podcast**: No Chicago food/location connections — not added
- **McDonald's Global Menu**: Rotates ~quarterly, 6 international items, 1035 W Randolph at HQ
- **Game Show Battle Rooms**: Lombard, 5 min from Glen Ellyn, book ahead weekends
- **Ethiopian dining**: Dan noted Nikki responds to sensory/communal experience — enhanced Demera entry

## Git Log
```
bf1bf2b Add ? button in header to revisit welcome screen
5c83d88 Google Places bottom sheet, trail map filter, map load fix
d8423f0 Theme trails, welcome screen, 70+ curated spots for Nikki
f7f1b28 Inject Mapbox token from GitHub secret during deploy
5e8f731 Add GitHub Pages deployment workflow
1ece644 Initial Chicago Explorer app — off-beat eats & adventures
2cefed0 Add favorites with localStorage persistence
```
