# Fling Punk - Development Progress

## Game Concept
A physics-based game where you swing a CryptoPunk head on a string and fling it as high as possible. The camera follows the punk as it flies upward, with height markers tracking your progress.

---

## Completed Features

### Core Gameplay
- **Pendulum Physics**: Punk head attached to anchor point via Matter.js constraint
- **Swing Mechanics**: Click/tap and drag to apply force to the swinging punk
- **Fling Release**: Release to detach from string and launch the punk upward
- **Velocity Boost**: Extra momentum applied on release (1.3x horizontal, scaled min upward speed + swing bonus)
- **Wall Boundaries**: Left and right walls keep the punk contained and allow bouncing
- **Power-ups**: Mushroom (50%, +10 boost), Flower (35%, +12 boost), Star (15%, +15 boost) — 8 per 100m
- **Dynamic power-up generation**: Power-ups generated ahead of the punk as it climbs, with cleanup of old ones

### Camera System
- **Follow Camera**: Camera tracks the punk vertically, keeping it centered on screen
- **Instant Follow**: Camera moves immediately with punk position for smooth tracking

### Visual Style
- **Primary Color**: Pink (#E56399)
- **Font**: Press Start 2P (retro pixel arcade style)
- **Pixelated Rendering**: `image-rendering: pixelated` for crisp pixel art
- **Background Punk**: Large punk image behind game area
- **Clouds**: Two cloud types with random scale, spread throughout the sky
- **Ground tiles**: Mario-style ground tiles at the base
- **Trail effect**: Pink semi-transparent trail follows punk during flight

### Random Punk Generation
- **Trait System**: Uses `/assets/traits/` folder with base characters and traits
- **Base Types**: alien, ape, zombie, male1-4, female1-4
- **Random Traits**: 1-3 random traits selected per punk (glasses, hats, hair, etc.)
- **Composite Rendering**: Base + traits layered on canvas for final punk image

### UI Elements
- **Title**: "FLING PUNK" header on start modal
- **Instructions**: "CLICK TO SWING - RELEASE TO FLING!"
- **Height Display**: Current max height in meters (top right)
- **Best Score**: Persisted to localStorage
- **Start Modal**: Shows punk preview with start button
- **Game Over Modal**: Shows final score, new record indicator, "PLAY AGAIN" button

### Height Tracking
- **Meter Markers**: Left side of screen shows height markers every 10m
- **Major Markers**: Highlighted at 50m and 100m intervals

### Play.fun (OpenGameProtocol) Integration
- **SDK**: Play.fun browser SDK loaded via CDN (`https://sdk.play.fun/latest`)
- **OGP Meta Tag**: `<meta name="x-ogp-key" content="64ee7b61-3b07-437b-bcea-565ee94a0ddc">`
- **Points Widget**: Enabled via `usePointsWidget: true` — shows built-in Play.fun points UI
- **Points System**: 1 point per meter of max height, awarded and saved on game over via `sdk.addPoints()` + `sdk.savePoints()`
- **Leaderboard**: Handled by Play.fun's infrastructure (replaced custom mann.cool/api leaderboard)
- Points accumulate across sessions

### Responsive Design
- **Full viewport**: Game fills 100vw x 100vh
- **9:16 aspect cap on desktop**: `max-width: calc(100vh * 9/16)` prevents desktop players from having a wider play area advantage over mobile
- **Dynamic scaling**: All game constants scale proportionally based on viewport vs design base (400x700)
  - `SCALE = Math.min(CANVAS_WIDTH / BASE_WIDTH, CANVAS_HEIGHT / BASE_HEIGHT)`
  - Punk size, string length, pixels per meter, power-up sizes, ground tiles, launch velocities, boosts — all scaled
  - `ANCHOR_Y` = 53% of viewport height (always positions correctly regardless of screen size)
- **Responsive text**: All HUD and modal fonts use `clamp()` for size
- **Resize handling**: `window.resize` listener + recalculation at each game start
- **Container-aware sizing**: Canvas reads from `container.clientWidth/Height` (respects CSS constraints)

---

## Technical Details

### Physics Configuration
```
Design base: 400x700 (all values scale from this)
SCALE: Math.min(viewportW/400, viewportH/700)

PUNK_SIZE: 64 * SCALE
STRING_LENGTH: 150 * SCALE
GRAVITY: 0.8 (fixed — Matter.js handles scaling)
SWING_FORCE: 0.002 (fixed)
PIXELS_PER_METER: 40 * SCALE
ANCHOR_Y: CANVAS_HEIGHT * 0.53
POWER_UP_DENSITY: 8 per 100 meters

Punk body:
- restitution: 0.5
- frictionAir: 0.001
- density: 0.003

Launch:
- minUpwardSpeed: 15 * SCALE
- maxSpeed: 35 * SCALE
- horizontalBoost: 1.3x current velocity
```

### File Structure
```
fling-punk/
├── index.html          # Main game file (single-file architecture)
├── PROGRESS.md         # This file
├── agents.md           # Symlink to shared agents documentation
├── GAME_INTEGRATION.md # Symlink to mann.cool integration docs
├── games.json          # Symlink to game registry
└── assets/
    ├── punks.png       # Background image
    ├── cloud.png       # Cloud sprite 1
    ├── cloud1.png      # Cloud sprite 2
    ├── ground-tile.png # Mario-style ground tile
    ├── mushroom.png    # Power-up: mushroom
    ├── flower.png      # Power-up: flower
    ├── star.png        # Power-up: star
    └── traits/
        ├── manifest.json   # List of all base types and traits
        ├── base/           # Base character PNGs (alien, ape, zombie, male/female)
        ├── m/              # Male trait PNGs (71 options)
        └── f/              # Female trait PNGs (76 options)
```

### Dependencies
- **Matter.js 0.19.0**: Physics engine (CDN)
- **Play.fun SDK**: Points/leaderboard (CDN)
- **Press Start 2P**: Google Font (CDN)

### Deployment
- **Repo**: github.com/songadaymann/punk-fling
- **Hosting**: Vercel (auto-deploys on push to `main`)
- **Access**: Proxied via mann.cool/punkfling (Vercel rewrite on the mann.cool side)
- All local asset paths are relative (works correctly through the rewrite proxy)

---

## Session History

### Session 1 (Initial Build)
1. Explored reference games (fill-the-punks, tower-of-punks) to understand style patterns
2. Created base game structure with HTML, CSS, and Matter.js setup
3. Implemented pendulum physics with anchor point and string constraint
4. Added swing mechanics with mouse/touch force application
5. Implemented release and fling with velocity boost on detachment
6. Built camera follow system to track punk vertically
7. Added height markers on left side of screen
8. Created random punk generation from trait assets
9. Added trail effect during flight
10. Fixed variable naming conflict (`render` was declared twice)
11. Added wall boundaries to contain punk horizontally
12. Updated visual style — pink background, white ground, background punk image

### Session 2 (Power-ups, Ground Tiles, Leaderboard)
- Added power-ups (mushroom, flower, star) with weighted probabilities and upward boosts
- Added Mario-style ground tiles
- Added leaderboard (mann.cool/api)
- Dynamic infinite power-up generation as punk climbs

### Session 3 (Play.fun Integration + Responsive) — 2026-02-06
1. **Play.fun SDK integration**: Added browser SDK, OGP meta tag, points widget
2. **Points system**: 1 point per meter of max height, saved on game over
3. **Removed custom leaderboard**: Stripped all mann.cool/api leaderboard code, HTML, CSS, and submit UI (188 lines removed)
4. **Simplified game-over modal**: Just score + "PLAY AGAIN" button now
5. **Power-up density**: Reverted from 11 back to 8 per 100 meters
6. **Full responsive redesign**:
   - Game container fills viewport (100vw x 100vh)
   - 9:16 aspect ratio cap on desktop via `max-width: calc(100vh * 9/16)`
   - Dynamic SCALE factor based on viewport vs 400x700 design base
   - All game constants (sizes, velocities, positions) scale proportionally
   - ANCHOR_Y tied to 53% of viewport height (not absolute pixels)
   - Responsive CSS text with `clamp()`
   - Resize handler + recalculation at each game start
   - Clouds regenerated at game start to use current scale

---

## Known Issues / Future Improvements

- [ ] Could add sound effects
- [ ] Could add particle effects on wall bounce or power-up collection
- [ ] Could tune physics for better "feel"
- [ ] Could add server-side score validation (Play.fun server SDK + Vercel serverless function) for anti-cheat
- [ ] Could add Play.fun token launch when ready
