# QA Checklist

## Structure

- [ ] 8–15 parts, each with a stable `id`
- [ ] Every part has a group, anchor, and explode offset
- [ ] Part list, labels, and detail panel all read the same table
- [ ] No duplicate IDs; no orphan meshes

## 3D

- [ ] Camera 3/4 front; model not cropped
- [ ] Model centered; side space for labels
- [ ] Materials consistent; no accidental pure-white/black surfaces
- [ ] Repeated arms/motors/props reuse geometry
- [ ] No visible gaps at assembled state
- [ ] Exploded state reads clearly; parts do not interpenetrate badly

## Interaction

- [ ] Orbit drag works; no stuck pointer state
- [ ] Wheel/pinch zoom within limits
- [ ] Click a part selects the correct detail
- [ ] Click a label selects the correct part
- [ ] Click a list row selects the correct part
- [ ] Single part toggles open/closed
- [ ] Explode all / reassemble all work
- [ ] Auto-rotate starts/stops
- [ ] Reset view returns to the authored framing
- [ ] Progress count 00/09 updates

## Labels

- [ ] Desktop labels do not overlap
- [ ] Labels stay inside the safe area
- [ ] Lines point to the correct anchors
- [ ] Active label is visibly different
- [ ] Mobile shows only the active label
- [ ] Label text does not overflow its box

## Responsive

- [ ] 1576×808
- [ ] 1366×768
- [ ] 1024×768
- [ ] 390×844
- [ ] No horizontal scroll
- [ ] Drawers open and close
- [ ] Tap targets ≥ 28px
- [ ] Touch orbit and pinch work

## Data and copy

- [ ] Official specs match the source
- [ ] Model-design values are marked
- [ ] Concept/artistic internal structure is marked
- [ ] Non-official disclaimer is present
- [ ] No invented materials/dimensions

## Delivery

- [ ] Source HTML/CSS/JS present
- [ ] Three.js is local
- [ ] Single-file build opens by double-click
- [ ] Console is clean
- [ ] Preview images included
- [ ] README lists controls, parts, data sources, disclaimer
