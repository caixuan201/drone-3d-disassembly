---
name: drone-3d-disassembly
description: Build a procedural Three.js 3D product exploded-view page with orbit/zoom, per-part separation, projected labels, detail panel, and responsive layout. Use for drones, hardware, engines, robots, or similar products. Includes a generic quadcopter case, part schema, code patterns, prompts, and QA.
---

# Product 3D Disassembly

Use this skill when the user wants a **single-page 3D product structure / exploded-view experience**: rotate, zoom, click a part, separate it, read details, and reassemble.

The reference case is a **generic 9-part quadcopter**, but the workflow applies to any hard-surface product with 8–15 meaningful modules.

## Non-negotiables

1. **Structure before code.** Define the part hierarchy and data table first.
2. **One part = one independent object/group.** Each part must be selectable and separable.
3. **The page is not a video.** It must support real orbit, zoom, click, and state changes.
4. **Labels must not fight the model.** Desktop shows the full set; mobile shows only the active part.
5. **Data must be honest.** Public specs get a source; internal structure is labeled as artistic/concept.
6. **Self-contained delivery.** Prefer procedural geometry; no external model, texture, image, or font dependency unless explicitly allowed.
7. **Brand-neutral examples.** Use generic product names and placeholder specs in the skill; the user supplies the real product and data.

Out of scope: non-page assets. This skill builds the interactive page only.

## Recommended inputs

- Product name, version, and one-sentence role.
- 8–15 part names, English IDs, and hierarchy.
- 3–5 specs per part, with source or `concept`.
- 3/4 reference image or 5-view board.
- Brand palette and desired UI tone.
- Target: desktop-first, mobile-first, or both.
- Whether an existing GLB is available.

If the user only has one image, do not promise a 360° faithful model. Offer a stylized concept build or a 2D/2.5D alternative.

## Workflow

### 1. Define the part graph

- 8–15 modules max.
- Every module gets a stable kebab-case `id`.
- Decide parent/child relationships and explode direction.
- Keep a **primary set** of 6–9 labels for desktop; defer the rest to the list/active state.

### 2. Write the parts table

Use `references/parts-schema.md`. Required fields:

```text
id, num, name, en, category, description,
specs[{k,v,source}], related[], anchor3d[3], explode3d[3], labelSide?
```

Do this before touching Three.js. The table drives the list, labels, detail panel, and animations.

### 3. Build the scene

- Three.js UMD, no CDN at runtime.
- Perspective camera, 3/4 front view, FOV ~38° desktop.
- Key + rim + fill lights; soft contact shadow; subtle fog; grid/technical floor.
- Camera distance must leave room for side labels; do not fill the frame with the model.
- Dark aerospace HUD is a safe default: blue-gray metal, cyan status, restrained warm accent.

### 4. Build the model

Default route: **procedural hard-surface geometry**.

- Rounded boxes, cylinders, lathes, extrusions, thin plates.
- Reuse geometry/materials for repeated arms, motors, props.
- Keep a consistent material language: metal, plastic, glass, rubber.
- Add small functional cues (joints, vents, lens rings, contacts) rather than random detail.
- Group by part ID; never join the whole product into one mesh.

If a clean GLB exists, use it but preserve the same part IDs and interaction contract. Do not silently swap a procedural claim for an external asset.

### 5. Build the explode system

Each part stores:

```text
home: Vector3
offset: Vector3
value: 0..1
target: 0 or 1
```

Every frame:

```text
value += (target - value) * (1 - exp(-k * dt))
position = home + offset * ease(value)
```

- Click a part: toggle its target.
- Explode all: set every target to 1.
- Reassemble: set every target to 0.
- Do not reset the user's camera on a single-part toggle.
- Camera may move to a side view only for “explode all” if that improves readability.

### 6. Build the labels

- Project each `anchor3d` to screen space every frame.
- Draw HTML labels and SVG leader lines over the canvas.
- Sort by projected Y and resolve collisions with a minimum gap.
- Desktop: full primary set on left/right columns.
- Mobile: active label only.
- Label click = same `select(id)` as model click and list click.

### 7. Build the UI

Six-block skeleton:

```text
top brand/status
left part list
center 3D
right detail panel
bottom public specs
3D callout labels
```

Add loading state, guide modal, toast, progress 00/N, explode all/reassemble, auto-rotate, reset view.

### 8. Responsive

- `>1160px`: three columns.
- `760–1160px`: side panels become drawers.
- `<760px`: single column, active label only, touch orbit/pinch.
- Keep tap targets ≥ 28px.
- Respect `prefers-reduced-motion`.

### 9. QA

Use `references/qa.md`. Test at 1576×808, 1366×768, 1024×768, 390×844. Check console, labels, drawers, touch, data, and disclaimer.

## Data integrity

- Public specs: cite the official source in copy or README.
- Internal structure: write “artistic/concept” and “non-official” explicitly.
- Do not invent materials, dimensions, or part counts to look precise.
- A short disclaimer is part of the design, not an afterthought.

## Delivery

- `index.html`, `styles.css`, `app.js`, `vendor/three.min.js`
- single-file build script
- preview images
- README with controls, parts, data sources, and disclaimer
- source files kept alongside the single-file build

## References

- `references/parts-schema.md` — JSON schema and a filled example
- `references/product-case.md` — 9-part generic quadcopter case, visual notes, spec placeholders
- `references/code-patterns.md` — scene, explode, labels, select, responsive, build
- `references/prompts.md` — asset, model, code, data, and QA prompts
- `references/qa.md` — acceptance checklist

Read only the reference needed for the current step. Do not load all references by default.
