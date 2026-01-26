# Fling Punk - Development Progress

## Game Concept
A physics-based game where you swing a CryptoPunk head on a string and fling it as high as possible. The camera follows the punk as it flies upward, with height markers tracking your progress.

---

## Completed Features

### Core Gameplay
- **Pendulum Physics**: Punk head attached to anchor point via Matter.js constraint
- **Swing Mechanics**: Click/tap and drag to apply force to the swinging punk
- **Fling Release**: Release to detach from string and launch the punk upward
- **Velocity Boost**: Extra momentum applied on release (1.3x horizontal, 1.5x vertical, with additional boost for upward swings)
- **Wall Boundaries**: Left and right walls (50,000px tall) keep the punk contained and allow bouncing

### Camera System
- **Follow Camera**: Camera tracks the punk vertically, keeping it centered on screen
- **Instant Follow**: Camera moves immediately with punk position for smooth tracking

### Visual Style (Matching Other Games)
- **Canvas Size**: 400x700px (mobile-friendly 9:16 aspect ratio)
- **Primary Color**: Pink (#E56399)
- **Font**: Press Start 2P (retro pixel arcade style)
- **Pixelated Rendering**: `image-rendering: pixelated` for crisp pixel art
- **Pink Background**: Body and game container use pink theme
- **White Ground**: Ground/start area is white with pink accents
- **Background Punk**: Large (300x300px) punk image at 30% opacity behind game area

### Random Punk Generation
- **Trait System**: Uses `/assets/traits/` folder with base characters and traits
- **Base Types**: alien, ape, zombie, male1-4, female1-4
- **Random Traits**: 1-3 random traits selected per punk (glasses, hats, hair, etc.)
- **Composite Rendering**: Base + traits layered on canvas for final punk image

### UI Elements
- **Title**: "FLING PUNK" header
- **Instructions**: "CLICK & DRAG TO SWING - RELEASE TO FLING!"
- **Height Display**: Current max height in meters (top right)
- **Best Score**: Persisted to localStorage
- **Start Modal**: Shows punk preview with start button
- **Game Over Modal**: Shows final score, new record indicator, restart button

### Height Tracking
- **Meter Markers**: Left side of screen shows height markers every 10m
- **Major Markers**: Highlighted at 50m and 100m intervals
- **Real-time Display**: Height updates as punk flies

### Trail Effect
- **Flight Trail**: Pink semi-transparent trail follows punk during flight (last 50 positions)

---

## Technical Details

### Physics Configuration
```javascript
CANVAS_WIDTH: 400
CANVAS_HEIGHT: 700
PUNK_SIZE: 64
STRING_LENGTH: 150
GRAVITY: 0.8
SWING_FORCE: 0.002
PIXELS_PER_METER: 40
ANCHOR_Y: 250

Punk body:
- restitution: 0.5 (bouncy)
- frictionAir: 0.001
- density: 0.003

Walls:
- restitution: 0.8 (bouncy walls)
- 50,000px tall
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
    └── traits/
        ├── manifest.json   # List of all base types and traits
        ├── base/           # Base character PNGs (alien, ape, zombie, male/female)
        ├── m/              # Male trait PNGs (71 options)
        └── f/              # Female trait PNGs (76 options)
```

### Dependencies
- **Matter.js 0.19.0**: Physics engine (CDN)
- **Press Start 2P**: Google Font (CDN)

---

## Session History

1. **Explored reference games** (fill-the-punks, tower-of-punks) to understand style patterns
2. **Created base game structure** with HTML, CSS, and Matter.js setup
3. **Implemented pendulum physics** with anchor point and string constraint
4. **Added swing mechanics** with mouse/touch force application
5. **Implemented release and fling** with velocity boost on detachment
6. **Built camera follow system** to track punk vertically
7. **Added height markers** on left side of screen
8. **Created random punk generation** from trait assets
9. **Added trail effect** during flight
10. **Fixed variable naming conflict** (`render` was declared twice)
11. **Added wall boundaries** to contain punk horizontally
12. **Updated visual style** - pink background, white ground, background punk image

---

## Known Issues / Future Improvements

- [ ] Could add sound effects
- [ ] Could add particle effects on wall bounce
- [ ] Could add leaderboard integration (like other games)
- [ ] Could tune physics for better "feel"
- [ ] Could add power-ups or obstacles at certain heights
