# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
A simple 2D top-down "Apartment Exploration" mini-game built with Vanilla JavaScript and HTML5 Canvas in a single `index.html` file.

## Development
Open `index.html` directly in a browser (no build step).

## Technical Specifications

### Rendering
- Single `<canvas>` element (1200x800 recommended size)
- Use `requestAnimationFrame` for the game loop
- Draw walls as black rectangles using the coordinates provided below

### Player
- Rendered as a small circle/dot (radius ~16)
- Arrow key movement, smooth per-frame movement (~120–180 px/sec)
- Normalize diagonal speed
- Spawn: **(573, 640)** (center bottom of apartment)
- Keep player inside map bounds

### Items (invisible on the map)
- Two invisible interaction points (DO NOT draw them on canvas):
  - Key: **(1030, 160)**, emoji **🔑**
  - Eyes: **(615, 150)**, emoji **👀**
- Pickup when player is "very close" (distance threshold ~50px)
- Eyes collectible only after Key (preferred)

### HUD (top-right overlay)
- Show two emoji indicators: 🔑 and 👀
- On load: both greyed out (low opacity / grayscale)
- After pickup: bright + "glowing" (CSS text-shadow)
- Items must not be visible on the map, only in HUD

### Collision (rectangles)
- Use a `walls` array of axis-aligned rectangles representing black wall segments from the map
- Player cannot enter rectangles (circle vs AABB collision is fine)
- Use axis-separated movement for smooth sliding

### Wall Coordinates
Based on the apartment floor plan, the walls are:

```javascript
const walls = [
  // Outer perimeter walls
  {x: 100, y: 75, width: 1000, height: 8},     // Top wall
  {x: 1092, y: 75, width: 8, height: 650},     // Right wall
  {x: 100, y: 717, width: 1000, height: 8},    // Bottom wall
  {x: 100, y: 75, width: 8, height: 350},      // Left wall (upper part)
  {x: 100, y: 525, width: 8, height: 200},     // Left wall (lower part)
  
  // Left room walls
  {x: 100, y: 442, width: 280, height: 8},     // Bottom wall of left room
  {x: 372, y: 300, width: 8, height: 150},     // Right wall of left room
  
  // Center room walls
  {x: 380, y: 292, width: 390, height: 8},     // Top wall of center room
  {x: 762, y: 300, width: 8, height: 250},     // Right wall of center room
  {x: 510, y: 542, width: 260, height: 8},     // Bottom wall of center room
];
```

### Conditional Wall
- Kitchen entrance blocking wall disappears after collecting Key
- Implement as one rectangle in `walls` that is removed/disabled when `hasKey=true`
- The conditional wall (entrance to left room):
  - `{x: 100, y: 425, width: 8, height: 100}` (vertical segment between rooms)

### Win State
- After collecting Eyes: show win overlay
- Provide a Reset button: resets player, items, and restores conditional wall

## Acceptance Criteria
- On load: 🔑 and 👀 greyed out; items not drawn on map
- Player moves with arrow keys; collisions prevent passing through walls
- Picking Key lights 🔑 and unlocks kitchen entrance
- Picking Eyes lights 👀 and shows win overlay
- Reset restores initial state