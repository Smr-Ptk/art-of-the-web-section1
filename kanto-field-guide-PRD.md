# PRD: Kanto Field Guide — Static Web Publication

## Overview
A Game Boy-styled fan site / online publication exploring the Kanto region from Pokémon. The user navigates between 5 HTML pages via a central Town Map hub. Built with vanilla HTML, CSS, and light JS. Hosted on GitHub Pages.

**Timeline:** ~2 hours
**Hosting:** GitHub Pages (username.github.io/kanto-field-guide)
**Constraint:** No single page + assets > 5MB

---

## Visual Design System

### Game Boy Palette (use ONLY these 4 colors)
| Role | Hex | Usage |
|------|-----|-------|
| Darkest | `#0f380f` | Text, borders, primary elements |
| Dark | `#306230` | Secondary text, shadows, accents |
| Light | `#8bac0f` | Backgrounds, cards, containers |
| Lightest | `#9bbc0f` | Page background, highlights |

### Typography
- **Font:** "Press Start 2P" from Google Fonts (pixel font)
- **Fallback:** monospace
- **Base size:** 10px body text (this font renders large), 16-20px headings
- Import: `<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">`

### Global Style Rules
- All images should use `image-rendering: pixelated` for crisp pixel art scaling
- Borders: 4px solid #0f380f (Game Boy "screen border" feel)
- No border-radius anywhere (keep everything sharp/square)
- Cursor: `url('assets/images/cursor.png'), pointer` (optional: custom pokeball cursor)
- Max content width: 800px, centered
- Subtle scanline overlay on every page (CSS repeating-gradient, optional but adds flavor):
  ```css
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      transparent 0px, transparent 2px,
      rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px
    );
    pointer-events: none;
    z-index: 9999;
  }
  ```

---

## File Structure

```
kanto-field-guide/
├── index.html              # Town Map (hub/nav page)
├── pallet-town.html        # Page 2: Origin story
├── route1.html             # Page 3: Wild encounters (vertical scroll)
├── pewter-gym.html         # Page 4: Gym battle
├── pokedex.html            # Page 5: Pokédex grid (FLEXBOX REQUIREMENT)
├── css/
│   └── style.css           # Single shared stylesheet
├── assets/
│   ├── images/
│   │   ├── town-map.png    # Kanto map image (can source from fan wikis or draw simple one)
│   │   ├── trainer.png     # Small trainer sprite
│   │   └── oak.png         # Professor Oak sprite (optional)
│   └── sprites/            # Pokémon sprites (download from PokeAPI or use URLs)
│       ├── bulbasaur.png
│       ├── charmander.png
│       ├── squirtle.png
│       └── ... (keep to ~15-20 sprites max)
└── README.md
```

### Asset Strategy
- **Pokémon sprites:** Use PokeAPI sprite URLs directly to save space: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/1.png` (Bulbasaur = 1, Charmander = 4, Squirtle = 7, etc.)
- **Town Map:** Either find a simple Kanto map PNG or create a minimal CSS-drawn version
- **Keep total assets under 5MB per page** — sprites are tiny (~3-5KB each), so this is easy

---

## Shared Components (in style.css)

### Persistent Navigation Bar
Every page (except index.html, which IS the nav) gets a bottom nav bar styled as a Game Boy "trainer menu":

```
┌─────────────────────────────┐
│ 🗺️ MAP | 📍 PALLET | 🌿 ROUTE 1 | ⚔️ GYM | 📖 POKÉDEX │
└─────────────────────────────┘
```

- Styled as a dark bar at the bottom of the viewport (fixed position)
- Current page link is highlighted (lighter green)
- Text-only, no emoji in production — use text labels styled in the pixel font
- This satisfies the "navigational toolset" requirement

### Page Transition Feel
- Each page link wraps content in a subtle fade-in CSS animation (0.3s)
- Optional: brief "screen flash" white overlay on page load (mimics Game Boy screen transition)

---

## Page Specifications

### PAGE 1: Town Map — `index.html`
**Purpose:** Hub page / primary navigation system
**Rubric:** Navigation system, sequence/flow, visual style

**Layout:**
- Full-viewport Game Boy "screen" frame (thick dark green border, rounded inner screen area)
- Kanto map image centered inside the screen
- Clickable hotspot `<a>` elements positioned absolutely over 4 locations on the map:
  - Pallet Town (bottom-center)
  - Route 1 (middle)
  - Pewter City (upper-left area)
  - Pokédex (as a "START menu" button below the map, separate from geography)
- On hover, each hotspot shows a pulsing dotted border or a small trainer sprite appears
- Title: "KANTO FIELD GUIDE" above the map in large pixel text
- Subtitle: "A Trainer's Publication — Select a Destination" below

**Implementation Notes:**
- Map container: `position: relative`
- Hotspots: `position: absolute` with `top/left` percentages so they scale
- Each hotspot is a styled `<a>` tag with a translucent highlight and label

**Sequence Design:**
- Visual arrows or numbered labels (①②③④) suggest the intended reading order: Pallet → Route 1 → Pewter Gym → Pokédex
- But the user can navigate freely (non-linear)

---

### PAGE 2: Pallet Town — `pallet-town.html`
**Purpose:** Introduction / origin story / atmospheric opener
**Rubric:** Creative format, cadence/rhythm, visual style

**Layout:**
- Styled as a series of Game Boy dialogue boxes
- The page is a vertical sequence of "text boxes" that appear like in-game dialogue:
  ```
  ┌──────────────────────────────────┐
  │ PROF. OAK: Welcome to the world  │
  │ of POKÉMON!                       │
  │                              ▼    │
  └──────────────────────────────────┘
  ```
- Each box contains 2-3 lines of text about Pallet Town, the journey beginning, choosing a starter, etc.
- Between boxes: small pixel art dividers or scene descriptions in italics
- Starter Pokémon showcase: 3 columns (Bulbasaur / Charmander / Squirtle) with sprites and short flavor text
- The "▼" triangle at the bottom-right of each box is a nice authentic touch (CSS triangle)

**Content (write in-character as a field guide narrator):**
- Welcome / intro to Kanto
- Professor Oak's Lab description
- The three starters with brief profiles
- "Your journey begins here. Head north to Route 1."

**Sequence Notes:**
- Slow pacing — lots of whitespace, boxes spaced out
- Mimics the deliberate, text-heavy opening of the games
- Ends with a clear prompt to visit Route 1 next (link styled as dialogue option: "> HEAD TO ROUTE 1")

---

### PAGE 3: Route 1 — `route1.html`
**Purpose:** Dynamic scrolling experience / "walking" through tall grass
**Rubric:** Cadence/rhythm/irregularity, creative format

**Layout:**
- Tall, vertically scrolling page
- Background: repeating grass tile pattern (CSS repeating background or simple green stripes)
- As the user scrolls, they encounter "wild Pokémon" — styled info cards that appear at intervals:
  ```
  ┌─────────────────┐
  │  Wild PIDGEY     │
  │  appeared!       │
  │  [sprite]        │
  │  Lv. 3 | Normal/Flying │
  │  "The most common│
  │   sight on any   │
  │   route."        │
  └─────────────────┘
  ```
- Cards alternate left/right alignment for visual rhythm
- Between encounters: stretches of "empty" grass (just the background) creating pause/breathing room
- Occasional "landmark" breaks: a sign post, a ledge, an NPC saying something

**Pokémon to feature (5-6):**
- Pidgey (#16), Rattata (#19), Spearow (#21), Oddish (#43), Mankey (#56), Pikachu (#25, rare — styled differently to show rarity)

**Sequence Notes:**
- Rhythm: encounter → pause → encounter → pause → surprise (Pikachu)
- The irregularity of the rare encounter breaks the pattern intentionally
- Ends with "You've reached Pewter City!" prompt linking to the gym page

---

### PAGE 4: Pewter City Gym — `pewter-gym.html`
**Purpose:** Dramatic focal point / "battle" page
**Rubric:** Creative format, punctuation moment in sequence

**Layout:**
- Styled as a battle screen from the Game Boy games
- Top section: "GYM LEADER BROCK wants to fight!" announcement banner
- Battle layout (two-panel):
  ```
  ┌─────────────────────────────────┐
  │         BROCK's ONIX            │
  │         [large sprite]          │
  │         Lv. 14 | Rock/Ground    │
  │    ┌─────────────────────┐      │
  │    │ HP ████████░░░ 70%  │      │
  │    └─────────────────────┘      │
  ├─────────────────────────────────┤
  │  YOUR POKÉMON                   │
  │  [trainer sprite / starter]     │
  │    ┌─────────────────────┐      │
  │    │ HP ██████████░ 90%  │      │
  │    └─────────────────────┘      │
  └─────────────────────────────────┘
  ```
- Below the battle screen: Brock's full team info (Geodude + Onix), badge info, strategy tips
- HP bars are simple CSS `<div>` bars with width percentages and the green palette colors
- BOULDER BADGE section at bottom: image/icon of the badge, what it unlocks

**Content:**
- Brock's team details
- Type matchup tips (use Water or Grass types!)
- The Boulder Badge description
- "With your first badge, you're ready to catalog what you've seen." → link to Pokédex

**Sequence Notes:**
- This is the climax / punctuation of the journey
- Tighter, more structured layout than Route 1's flowing scroll
- Feels like arriving at a destination

---

### PAGE 5: Pokédex — `pokedex.html`
**Purpose:** Reference / catalog page
**Rubric:** ⭐ GRID REQUIREMENT (flexbox), visual style

**Layout:**
- Header: "POKÉDEX" title, subtitle: "Pokémon encountered on your journey"
- **Flexbox grid** of Pokémon cards, 3-4 per row, wrapping responsively
- Each card:
  ```
  ┌──────────────┐
  │   [sprite]   │
  │  #025        │
  │  PIKACHU     │
  │  Electric    │
  │              │
  │  "It stores  │
  │  electricity │
  │  in its      │
  │  cheeks."    │
  └──────────────┘
  ```
- Cards have the dark border, lighter background
- Type badges: small colored labels (even within the green palette — use darkest for certain types, lighter for others, or use subtle patterns)

**Flexbox Implementation:**
```css
.pokedex-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  justify-content: center;
}

.pokedex-card {
  flex: 0 1 200px;       /* don't grow, can shrink, base 200px */
  border: 4px solid #0f380f;
  background: #8bac0f;
  padding: 16px;
  text-align: center;
}
```

**Pokémon to include (12-15 cards):**
All Pokémon mentioned across the site plus a few extras:
- #001 Bulbasaur, #004 Charmander, #007 Squirtle (starters)
- #016 Pidgey, #019 Rattata, #021 Spearow, #025 Pikachu, #043 Oddish, #056 Mankey (Route 1)
- #074 Geodude, #095 Onix (Brock's team)
- Add 2-3 more for variety: #129 Magikarp, #143 Snorlax, #150 Mewtwo

**Sequence Notes:**
- This is the epilogue / appendix
- Encyclopedic, browsable, non-linear
- Satisfying "collection" feeling
- Links back to Map at bottom

---

## Rubric Checklist

| Requirement | Where it's fulfilled |
|---|---|
| Consistent visual style (10 pts) | Game Boy 4-color palette, Press Start 2P font, shared CSS, border style, scanlines — every page uses the same design system |
| Creative webpage format (10 pts) | Each page mimics a different Game Boy UI: map screen, dialogue boxes, tall-grass scrolling, battle screen, Pokédex catalog |
| Navigation & sequence principles (10 pts) | Town Map hub with suggested order (numbered). Persistent bottom nav on all subpages. Cadence: slow intro → rhythmic route → dramatic gym → browsable pokédex. End-of-page prompts guide to next page. |
| Grid layout on ≥1 page (10 pts) | Pokédex page uses CSS flexbox grid |
| File/directory organization (5 pts) | Clean structure: css/, assets/images/, assets/sprites/, pages at root |
| Critique navigation walkthrough (5 pts) | Walk through: Map → Pallet Town → Route 1 → Pewter Gym → Pokédex |

---

## Build Order (for 2-hour timeline)

### Phase 1: Scaffold (15 min)
1. Create repo, file structure, all 5 empty HTML files
2. Set up `style.css` with CSS reset, palette variables, font import, body styles, scanline overlay
3. Create the shared nav bar component (copy-paste into each page)
4. Test that GitHub Pages deploys

### Phase 2: Hub Page (20 min)
1. Build `index.html` Town Map layout
2. Either use a sourced Kanto map PNG or create a minimal CSS version (colored rectangles for towns, lines for routes)
3. Position clickable hotspots with absolute positioning
4. Style hover states

### Phase 3: Content Pages (60 min, ~15 min each)
1. **pallet-town.html** — Dialogue box CSS component, starter showcase (3-column)
2. **route1.html** — Tall scroll page, alternating encounter cards, grass background
3. **pewter-gym.html** — Battle screen layout, HP bars, badge section
4. **pokedex.html** — Flexbox grid, Pokémon cards with sprites from PokeAPI URLs

### Phase 4: Polish (15 min)
1. Add page-load fade-in animation
2. Check all links work
3. Ensure no page exceeds 5MB
4. Add end-of-page navigation prompts between pages
5. Test on mobile (basic responsiveness)
6. Push to GitHub, verify GitHub Pages deployment

### Phase 5: README (10 min)
1. Brief project description
2. Navigation guide for critique presentation

---

## GitHub Pages Deployment

1. Create repo: `kanto-field-guide` (public)
2. Push all files
3. Settings → Pages → Source: "Deploy from a branch" → Branch: `main`, folder: `/ (root)`
4. Site will be live at: `https://[username].github.io/kanto-field-guide/`

---

## Sprite Reference (PokeAPI URLs)

```
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/1.png    # Bulbasaur
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/4.png    # Charmander
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/7.png    # Squirtle
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/16.png   # Pidgey
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/19.png   # Rattata
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/21.png   # Spearow
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/25.png   # Pikachu
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/43.png   # Oddish
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/56.png   # Mankey
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/74.png   # Geodude
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/95.png   # Onix
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/129.png  # Magikarp
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/143.png  # Snorlax
https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/150.png  # Mewtwo
```

---

## Notes for Cursor AI
- Build one page at a time, test each before moving on
- Keep all CSS in the single shared stylesheet
- Use CSS custom properties for the palette so changes propagate everywhere
- The dialogue box and encounter card are reusable CSS components — define them once
- Don't over-engineer: this is static HTML/CSS with minimal JS (fade-in animation at most)
- When in doubt, keep it simple — the Game Boy aesthetic rewards minimalism
