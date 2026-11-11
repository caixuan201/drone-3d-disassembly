# Code Patterns

Minimal patterns for a single-page Three.js product disassembly build. Adapt names and geometry; keep the state model.

## 1. Page shell

```html
<canvas id="scene"></canvas>
<header id="topbar">...</header>
<aside id="partsPanel">...</aside>
<aside id="detailPanel">...</aside>
<section id="specStrip">...</section>
<div id="labelLayer"><svg id="leaderSvg"></svg><div id="labels"></div></div>
<script src="vendor/three.min.js"></script>
<script src="app.js"></script>
```

No external fonts, textures, or model files in the default route.

## 2. Scene and camera

```js
const scene = new THREE.Scene();
scene.fog = new THREE.FogExp2(0x04090c, 0.035);

const camera = new THREE.PerspectiveCamera(38, innerWidth / innerHeight, 0.1, 120);
const view = {
  theta: 2.5, phi: 1.15, radius: 12,
  targetTheta: 2.5, targetPhi: 1.15, targetRadius: 12,
  center: new THREE.Vector3(0, 0.15, 0)
};

function updateCamera(dt) {
  const k = 1 - Math.exp(-7 * dt);
  view.theta += (view.targetTheta - view.theta) * k;
  view.phi += (view.targetPhi - view.phi) * k;
  view.radius += (view.targetRadius - view.radius) * k;
  const sp = Math.sin(view.phi);
  camera.position.set(
    view.center.x + view.radius * sp * Math.sin(view.theta),
    view.center.y + view.radius * Math.cos(view.phi),
    view.center.z + view.radius * sp * Math.cos(view.theta)
  );
  camera.lookAt(view.center);
}
```

## 3. Part runtime

```js
const runtime = {};

function registerPart(part, group, anchor) {
  runtime[part.id] = {
    config: part,
    group,
    home: group.position.clone(),
    offset: new THREE.Vector3(...part.explode3d),
    value: 0,
    target: 0,
    anchor
  };
}

function select(id) {
  state.selected = id;
  updateActiveClasses();
  renderDetail(runtime[id].config);
}

function toggle(id) {
  const r = runtime[id];
  r.target = r.target > 0.5 ? 0 : 1;
  select(id);
}

function explodeAll() {
  Object.values(runtime).forEach(r => { r.target = 1; });
}

function reassembleAll() {
  Object.values(runtime).forEach(r => { r.target = 0; });
}
```

## 4. Explosion update

```js
function ease(t) {
  return t < 0.5 ? 4 * t * t * t : 1 - Math.pow(-2 * t + 2, 3) / 2;
}

function updateParts(dt) {
  for (const r of Object.values(runtime)) {
    r.value += (r.target - r.value) * (1 - Math.exp(-5.5 * dt));
    if (Math.abs(r.target - r.value) < 0.0005) r.value = r.target;
    const e = ease(r.value);
    r.group.position.copy(r.home).addScaledVector(r.offset, e);
  }
}
```

Do not bake the explosion into keyframes. The offset is data.

## 5. Raycasting

```js
const raycaster = new THREE.Raycaster();
const ndc = new THREE.Vector2();
const clickable = [];

function hit(clientX, clientY) {
  const r = renderer.domElement.getBoundingClientRect();
  ndc.x = ((clientX - r.left) / r.width) * 2 - 1;
  ndc.y = -((clientY - r.top) / r.height) * 2 + 1;
  raycaster.setFromCamera(ndc, camera);
  const hits = raycaster.intersectObjects(clickable, false);
  return hits.length ? hits[0].object.userData.partId : null;
}
```

Every mesh in a part gets `userData.partId = part.id` and is added to `clickable`.

## 6. Labels

```js
function updateLabels() {
  const w = innerWidth, h = innerHeight;
  const svg = document.getElementById('leaderSvg');
  svg.setAttribute('viewBox', `0 0 ${w} ${h}`);

  for (const r of Object.values(runtime)) {
    r.anchor.getWorldPosition(tmp).project(camera);
    const x = (tmp.x * 0.5 + 0.5) * w;
    const y = (-tmp.y * 0.5 + 0.5) * h;
    // hide off-screen; choose side by x; stack labels with a min gap
    // set label transform + SVG path from label edge to (x,y)
  }
}
```

Label strategy:

- Desktop: 9 primary labels, left/right columns.
- Tablet: reduce to 6 primary labels.
- Mobile: active label only.
- On resize, recalculate positions; do not cache screen coordinates.

## 7. Select from model, label, and list

All three entry points call the same function:

```js
function selectPart(id, fromMobile) {
  state.selected = id;
  markVisited(id);
  updateActiveClasses();
  renderDetail(PARTS_BY_ID[id]);
  if (fromMobile && innerWidth <= 1160) openDetailDrawer();
}
```

Do not duplicate state logic in click handlers.

## 8. Responsive

```css
@media (max-width: 1160px) {
  .panel { position: fixed; transform: translateX(-100%); }
  .panel-right { transform: translateX(100%); }
  .panel.is-open { transform: translateX(0); }
}
@media (max-width: 760px) {
  .part-label { display: none; }
  .part-label.is-active { display: block; }
}
```

Test 1576, 1366, 1024, 390. At 390, no horizontal scroll and no hover-only controls.

## 9. Build script

```js
// build.mjs
const html = fs.readFileSync('index.html', 'utf8')
  .replace('<link rel="stylesheet" href="styles.css">', `<style>${css}</style>`)
  .replace('<script src="app.js"></script>', `<script>${js}</script>`)
  .replace('<script src="vendor/three.min.js"></script>', `<script>${three}</script>`);
fs.writeFileSync('product-3d.html', html);
```

Deliver both source files and the single-file build.

## 10. Performance

- `renderer.setPixelRatio(Math.min(devicePixelRatio, 2))`
- Reuse geometries/materials for repeated arms, motors, props.
- Keep shadows limited to the key light.
- Avoid postprocessing unless it changes the read.
- Target 60fps desktop, 30–60fps mobile.
- No console errors.
