# Floor Plan Interior Designer

[中文](README.md) | English

A pure front-end tool for interior design on a floor plan: place furniture, remove or modify walls, and take measurements on a 2D plan, then switch to a Three.js 3D scene with one click — view it from above or walk through it in first person. The floor plan itself is editable too: import a plan image to trace over, or reshape it with the mouse — drag walls, drag corners, draw walls and rooms. The whole app is a single `index.html`: no build step, just open it.

## Features

**2D Floor Plan**
- Displays the original floor plan at 1:60 / 1:100 scale, dimensions in mm
- Drag 60+ furniture and appliance items from the library on the left (bedroom, living room, dining & kitchen, bathroom, appliances, study & leisure)
- Move, rotate (hold Shift for free angle), and resize items, with automatic snapping to walls
- Measuring tool (snaps to nearby walls; hold Shift to lock horizontal / vertical)
- Remove or modify non-load-bearing walls; load-bearing walls are marked separately
- "Plan parts" library on the left: load-bearing / exterior / non-load-bearing / half walls, ready-made rooms, windows, bay windows, doors, and sliders — all drag-and-drop onto the plan
- A single "Snap" button toggles all snapping: walls and rooms snap to the grid, to existing wall lines, and to neighbouring rooms; grid spacing is selectable (10 / 50 / 100 / 250 / 500 mm, 1 m) and labelled with its unit
- Layer toggles: dimensions, room names, furniture, grid, load-bearing walls

**Editing the Floor Plan (mouse only)**
- Import a plan image: PNG / JPG / WebP, automatically fitted to the plan extent, with opacity, drag, corner-resize, and lock
- Two-point calibration: click twice on the image and type the real distance (mm); the image is rescaled to true size so you can trace over it
- Drag a dashed grip to move that axis — every wall, opening, and room edge aligned to it follows, so the plan stays orthogonal
- Drag a room corner (square) to move both the horizontal and the vertical axis through that point
- Drag a wall body to shift the whole segment; joined walls stretch and room edges follow
- Draw walls (drag a line, snaps to existing axes), draw rooms (rectangle or polygon, close with double-click / Enter), and create openings (drag out a window / door / slider along a wall — an opening never slides off its wall)
- Click any wall, window, door, slider, room, or the plan image to edit its type, size, and position in the right panel; flip a door hinge or swing, flip a slider's direction
- Select any wall, window, door, slider, or room and press `Delete` to remove it (undoable); the Select tool (`V`) also picks them up so you can drag their grips to resize
- Drag the round grip in the centre of a room to move the whole room: the ring of walls around it (and their openings) travels with it, **wall thickness and length stay exactly the same**, attached bay windows come along, and it stops automatically before hitting a neighbouring room — two rooms never overlap
- Drag the four corner handles (squares) of a room's bounding box to resize that room: the ring of walls stays flush with the room on all sides with unchanged thickness; the minimum is 600 mm and it cannot grow past a neighbouring room
- Furnish automatically: "🛋 Furnish" in the toolbar (or the same button in the right overview panel) lays out a full set per room type (bedroom / living / dining / kitchen / bathroom / laundry / balcony), and "Furnish this room" in the room panel does just the selected one; pieces hug the walls, avoid door openings and never poke through a wall — you can keep dragging them afterwards
- On first open (or whenever the saved scheme has no furniture yet) the layout is arranged automatically; a scheme you arranged yourself is never overwritten
- Drop parts from the "Plan parts" library: a dropped wall continues the direction and thickness of a nearby wall, a room's edges snap to nearby wall lines, and openings must land on a wall (a warning appears otherwise)
- Wall types: load-bearing / exterior / removable non-load-bearing / half wall; additions and deletions stay in sync with 3D and the dimension lines re-lay out automatically
- Every change goes on the undo stack, plans are auto-saved locally, and you can export / import them as JSON

**3D Scene**
- Bird's-eye, oblique, and top-down views; click a room in the list to fly to it, or **click the floating room-name label above a room in the 3D scene** to smoothly fly the camera there (the label highlights and the 2D selection syncs)
- Walkthrough mode: WASD + mouse on desktop, virtual joystick on touch devices; aim at a door and click, or press `E`, to open / close it — standing near a door is enough, so a door you closed can always be opened again
- Toggle between full-height and cut-away walls, time-of-day sunlight slider, night lighting
- Detailed furniture models: cabinet door gaps and handles, upholstered headboards, metal and ceramic materials with environment reflections, and more
- Select and drag furniture in 3D as well, kept in sync with the 2D plan in real time

**Plans & Statistics**
- Automatic calculation of room areas and net usable floor area
- Change the floor material of each room (wood, tiles, marble, terrazzo, carpet, etc.), with cost estimates based on area plus 5% wastage
- Undo / redo; plans are auto-saved in the browser's local storage
- Chinese / English UI toggle (button on the right of the top bar; defaults to Chinese and remembers your choice)
- Export to PNG, export / import plans as JSON
- **Export DXF for CAD (open it in CAD, then Save As DWG)**: walls / low walls / doors (with swing arcs) / windows / sliding doors / glass / furniture / room names and areas / dimensions / the imported plan image, organised on 12 layers with per-layer colours, 1:1 in millimetres, fully editable in CAD

### About DWG and the DXF export

- **Why not .dwg directly**: DWG is Autodesk's closed binary format; writing it requires a commercial SDK such as ODA Teigha / RealDWG, which a single offline HTML file cannot ship. **DXF is DWG's official interchange format**: AutoCAD, ZWCAD, GstarCAD, TArch, BricsCAD, LibreCAD and others open the exported `floor-plan.dxf` directly — then use **Save As → DWG**.
- The exported DXF uses the **2007 format (AC1021)**: since AutoCAD 2007, DXF text is UTF-8, so Chinese room / furniture / title text comes out as **real characters with no mojibake** in AutoCAD, ZWCAD, GstarCAD, BricsCAD and anything from 2007 onward.
- The File menu also offers **"Export DXF (R12, for very old CAD)"**: for software that only understands R12. That file is pure ASCII and writes Chinese as AutoCAD `\U+XXXX` escapes (`\U+4E3B\U+5367` = 主卧), independent of any code page. Use it only if the default export shows garbled text.
- The plan's y axis points down; the exported drawing is flipped to CAD's y-up, so it looks exactly like it does on screen.

## Quick Start

```bash
git clone <repository-url>
cd <repository-directory>
```

Then simply open `index.html` in your browser. Alternatively, start a local static server:

```bash
python3 -m http.server 8000
# Visit http://localhost:8000
```

> Three.js is loaded from the jsDelivr CDN, so an internet connection is required the first time you open the 3D scene.

## Keyboard Shortcuts

| Key | Action |
| --- | --- |
| `T` | Toggle 2D / 3D |
| `V` / `M` / `X` | Select / Measure / Modify walls (the Select tool also picks walls, rooms and openings) |
| `P` / `W` / `G` / `O` | Plan edit / Draw wall / Draw room / Create opening |
| `Enter` | Close the polygon room being drawn |
| `R` / `Shift+R` | Rotate selected furniture 90° clockwise / counterclockwise |
| `Delete` / `Backspace` | Delete the selected furniture, wall, opening, or room |
| `Ctrl/⌘ + D` | Duplicate selected furniture |
| `Ctrl/⌘ + Z`, `Ctrl/⌘ + Shift + Z` | Undo, redo |
| `F` | Fit to window |
| `+` / `-` | Zoom in / out |
| `[` / `]` | Show / hide the furniture library (left) and the side panel (right) |
| `Shift + F` | Fullscreen |
| `Esc` | Cancel current action |
| Walkthrough: `WASD` / arrow keys, `Shift`, `E` | Move, walk faster, open / close doors |

## Tech Stack

- Vanilla HTML / CSS / JavaScript — no framework, no build step
- 2D floor plan rendered with SVG
- 3D scene built with [Three.js](https://threejs.org/) r160 (OrbitControls, PointerLockControls, RoundedBoxGeometry, RoomEnvironment, CSS2DRenderer)
- Data stored in `localStorage`

## Editing Your Own Floor Plan

Two ways, neither requires touching code:

1. **Edit it in the app**: the "✎ Plan" tool in the top bar lets you drag wall axes, corners, and wall bodies by their grips; you can also drag load-bearing walls, exterior walls, rooms, windows, and doors straight from the "Plan parts" library onto the plan (they snap to the grid and to existing wall lines); "Draw wall / Draw room / Opening" fills in new structure, and `Delete` removes the selected part. The 3D scene rebuilds to match.
2. **Trace an imported plan image**: menu → "Import plan image", then calibrate the scale with two points before tracing walls, rooms, and openings on top of it.

Your edited plan is saved in the browser's local storage; "Export JSON" writes it to a file you can share or import on another machine.

## Customizing the Floor Plan (code)

The initial floor plan data lives in `index.html` (runtime edits are written back into the same structure):

- `ROOMS`: room polygons, names, and default floor materials
- `WALLS` / `WINS` / `DOORS` / `SLIDES`: walls, window openings, doors, sliding doors (runtime edits are saved back into `state.plan`)
- `MATS`: floor material names and unit prices
- `LIB`: furniture library (type, name, default size, color)
- `buildFurniture()`: 3D models for each furniture type

Edit this data to use your own floor plan.

## Social Media

- X (Twitter): [@akokoi1](https://x.com/akokoi1)

## License

[MIT](LICENSE)
