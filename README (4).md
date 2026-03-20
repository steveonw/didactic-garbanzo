# Staged 3D Modeler v4

A skill for building 3D scenes with AI — blockout-level architecture, interiors, villages, compounds, and props using Three.js. The AI handles spatial reasoning, template management, and code generation. You handle creative direction and approval.

## Quick Start

**Upload this skill folder to your AI conversation, then just describe what you want:**

> "Build me a medieval tavern interior with a bar area, kitchen, and three bedrooms upstairs. I want to walk through it."

> "Create a Japanese shrine compound with a ceremonial entrance, main hall, and quiet garden areas."

> "Make a small marketplace with market stalls around a central well and some traders."

The AI will decompose your description, define templates, place objects, validate the layout, and generate a single HTML file you can open in any browser. You can orbit the scene and walk through it at eye level.

## The Template Library

This is the most important thing to understand.

**Templates are reusable 3D objects.** A "chair" template is defined once with its shape, dimensions, and materials. Then it can be placed 50 times in a scene — each instance with its own position and rotation, but sharing the same definition. This is how a 63-object tavern gets built in minutes instead of hours.

### Using a pre-built library

If you have a `template_library.json` file (from a previous session or from someone else), upload it and say:

> "Here's my template library. Use these objects to build [your scene]."

The AI will load the library and use existing templates instead of defining new ones from scratch. A library with 24 templates (furniture, props, structural elements) saves 10-15 minutes of setup per scene.

### The starter library

This skill comes with 6 built-in defaults (house, person, tree, well, cart, market stall). But over the course of building scenes, we've grown a 24-template library that includes:

**Structural:** house, well, market stall
**Vehicles:** cart (with correctly rotated wheels)
**People/Nature:** person figure, tree
**Furniture:** bed, round table, chair, bar counter, wall shelf, chest, cooking table, nightstand
**Props:** barrel, mug, candle, bottle, wall torch, cooking pot, grain sack, candle holder, floor rug, plate

Ask the AI for "the library" or upload your `template_library.json` to get started with these.

### Growing the library

Every scene you build adds templates. Built a blacksmith? Now you have an anvil, a forge, and a tool rack. Built a chapel? Now you have pews, an altar, and stained glass frames. The library accumulates across sessions — just save the `template_library.json` at the end and upload it next time.

**Community idea:** Share your library files with others. Someone builds medieval furniture, someone else builds sci-fi consoles, someone else builds Japanese architecture. Combine the libraries and any AI has instant access to hundreds of pre-built objects.

## How a Scene Gets Built

You don't need to understand this in detail — just know the steps exist so you can steer the process.

### 1. You describe the scene
One sentence is enough. "A roadside inn with a welcoming front and a working back yard." The AI breaks it down from there.

### 2. The AI decomposes it
What objects are needed? What templates exist? What needs to be created? This is the planning step.

### 3. Templates get defined
New objects are built from simple shapes — boxes, cylinders, spheres, cones. Each template has a bounding box so the system knows how much space it takes up.

### 4. Objects get placed
The AI places objects on a grid. A spatial validator checks for overlaps, out-of-bounds placement, and floating objects in real time. If something overlaps, the validator catches it and the AI fixes it.

### 5. Verification
Four layers, each catching different problems:

- **Spatial validation** — overlaps, bounds, support (exact, automated)
- **Braille views** — silhouette sanity check, very cheap (automated)
- **Path walk** — "walk" through the scene as text, catch gaps and awkward spacing (automated)
- **Walk mode** — you walk through the scene at eye level in the browser (human, visual)

### 6. Code generation
The AI generates a single `.html` file. Open it in any browser. No installs, no dependencies, no build step.

## Walk Mode

Every generated scene has a built-in walk mode.

**Press F** (or click the Walk button) to switch from orbit view to first-person.

| Control | Action |
|---------|--------|
| W / ↑ | Walk forward |
| S / ↓ | Walk backward |
| A / ← | Strafe left |
| D / → | Strafe right |
| Mouse | Look around (click to lock cursor) |
| Q / E | Rotate left/right (keyboard, no mouse needed) |
| R / T | Look up/down |
| F | Toggle walk/orbit mode |
| Esc | Return to orbit |

The HUD at the bottom shows your position and compass direction.

## Tips for Getting Good Results

### Be specific about zones
Instead of "build a house," say "build a house with a cozy living room on the left, a kitchen in the back, and a bedroom upstairs." Zones give the AI spatial structure to work with.

### Mention what you want to feel
"The front should feel welcoming" or "the back should feel practical" tells the AI about mood, not just geometry. It affects spacing, object density, and material choices.

### Ask for verification
Say "run a walk check" or "show me the braille views" if you want to see the verification output. The AI can do this automatically, but sometimes it's useful to see the raw data.

### Use the walk mode early
Don't wait until the scene is "done" to walk through it. Walk through it after the first placement pass. You'll catch spacing issues, awkward entrances, and dead zones that no automated tool can see.

### Upload your library every time
If you built objects in a previous session, upload the `template_library.json`. Say "here's my library, use what fits." The AI will check what's available before defining new templates.

### Ask for individual placement when editing
If you plan to hand-edit the generated code, ask the AI to use `--individual` mode. This generates each object as its own named code block instead of batching them into loops. Easier to cut, paste, and tweak.

### Save your files
At the end of a session, save:
- `template_library.json` — your accumulated object library
- The generated `.html` file — your scene
- `scene.json` — the placement data (if you want to edit positions later)

## What This Skill Is Good At

- Blockout-level architecture (buildings, compounds, villages)
- Interior layouts with walkable rooms
- Scene composition with multiple zones
- Repeated elements (rows of stalls, fences, trees, furniture sets)
- Rapid iteration — change a placement, regenerate, check

## What This Skill Is Not

- Not a final-art tool. No textures, no UV mapping, no detailed meshes.
- Not for organic modeling (characters, creatures, terrain).
- Not for animation or rigging.
- Requires a code execution environment (Python scripts for validation and generation).

Think of it as a 3D whiteboard. Fast, editable, good enough to communicate spatial ideas and test layouts before committing to real production work.

## File Reference

| File | What it does |
|------|-------------|
| `template_library.json` | Your object library (upload this between sessions) |
| `scene.json` | Object positions and scene grid |
| `library.json` | Working copy of templates for current scene |
| `*.html` | Generated 3D scene (open in browser) |

### Scripts (the AI runs these, you don't need to)

| Script | Purpose |
|--------|---------|
| `template_library.py` | Define, place, and generate code from templates |
| `spatial_validate.py` | Check for overlaps, bounds, floating objects |
| `braille_view.py` | Cheap silhouette verification views |
| `path_walk.py` | Text-based walk through the scene |
| `scaffold.py` | Generate the HTML scaffold with walk mode |
| `design_napkin.py` | Track design decisions across the conversation |
| `worklog.py` | Log milestones and changes |

## Example Prompts

**Exterior scenes:**
- "Build a medieval marketplace with 6 stalls in a U-shape around a well"
- "Create a hilltop shrine compound with a ceremonial entrance and quiet garden"
- "Make a roadside inn with a welcoming front porch and a working stable in back"

**Interior scenes:**
- "Build a tavern interior with a bar, kitchen, and three bedrooms with door cutouts I can walk through"
- "Create a wizard's study with bookshelves, a desk, potion bottles, and a fireplace"
- "Make a medieval kitchen with prep tables, a hearting, storage barrels, and grain sacks"

**Using the library:**
- "Here's my template library [upload]. Build a banquet hall using the tables, chairs, and candles from it"
- "Load this library and show me what's available, then build a scene that uses at least 10 of these templates"
- "I have these objects [upload library]. Add 5 new templates for a blacksmith workshop and save the updated library"

**Iteration:**
- "The chairs are facing the wrong way, rotate them toward the tables"
- "The stairs feel too far from the gate, move them closer"
- "Add decorations to the bar — mugs on tables, bottles behind the counter, wall torches"
- "Run a walk check from the entrance through every room"

## Version

v4.0.7 — Staged 3D Modeler
94 tests passing. Built and tested across 4 scenes (marketplace, roadside inn, shrine compound, tavern interior).
