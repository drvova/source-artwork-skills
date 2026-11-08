---
name: source-artwork-inspection
description: Inspect a real page, application, or asset to locate the original artwork and animation source, observe rendered behavior, and produce evidence separated from inference.
---

# Source Artwork Inspection

## Purpose

Find what actually produces the visible artwork and motion. Deliver a compact evidence record for geometry, frame-data, animation, or 3D skills; do not redraw, animate, or score the result here.

## Activate When

- A website, app, game, or repository contains artwork to reproduce.
- A screenshot may hide the actual SVG, sprite, canvas, video, or model source.
- The source format, animation mechanism, interaction, or authoring tool is unknown.

## Workflow

1. **Capture the rendered reference.** Record URL/path, viewport, device scale, background, crop, dimensions, and timestamp. Inspect idle, hover, focus, pointer, scroll, responsive, and reduced-motion states that affect the asset.
2. **Locate source candidates.** Inspect supplied files and repository assets, rendered DOM, network-loaded resources, styles, scripts, source maps, metadata, and exports as available. Identify which asset and runtime path feed the visible result.
3. **Trace behavior separately.** Determine whether motion comes from path changes, CSS transforms, frame data, canvas, video, or a 3D scene. Record frame rate, loop/stop behavior, and interaction triggers only when observed or proven in code.
4. **Preserve provenance.** Keep an unmodified source copy when permitted. Record its exact URL or repository path, filename, dimensions, and checksum when available. Do not silently replace the source with a screenshot.
5. **Separate evidence classes.** Label each finding `confirmed`, `observed`, `inferred`, or `unknown`. A compiled SVG or packed frame format does not prove which authoring tool created it; report that tool as unknown unless source files, metadata, or explicit attribution establish it.
6. **Hand off a useful record.** Include source candidates, selected asset and rationale, capture conditions, geometry/frame hints, runtime behavior, provenance, unresolved questions, and links to saved captures. Use a concise Markdown or structured project-native record.

## Boundaries

- Do not decode custom frame formats; use [Sprite Frame Data](../source-artwork-sprite-frames/SKILL.md).
- Do not normalize or edit SVG/cell geometry; use [SVG and Pixel-Cell Geometry](../source-artwork-svg-cells/SKILL.md).
- Do not author motion, build 3D, or claim fidelity thresholds.

## Acceptance

- [ ] Source assets and runtime behavior are distinguished from rendered appearance.
- [ ] Capture conditions and provenance are recorded.
- [ ] Confirmed facts, observations, inferences, and unknowns are not conflated.
- [ ] Authoring-tool claims are supported or marked unknown.
- [ ] Downstream work can proceed without repeating the inspection.
