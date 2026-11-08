---
name: source-artwork-svg-cells
description: Preserve, validate, or regenerate SVG vector paths and pixel-cell geometry while retaining path order, fills, transforms, grid, palette, and registration.
---

# SVG and Pixel-Cell Geometry

## Purpose

Prepare faithful 2D source geometry for animation or 3D work. Own geometry parsing and preservation—not source discovery, motion authoring, or fidelity verdicts.

## Activate When

- Existing SVG paths or grouped vector artwork need inspection or reuse.
- Pixel-art cells are encoded as SVG rectangles, path segments, or grouped shapes.
- Geometry must be transformed into a validated representation without changing its visible source.

Use [Source Artwork Inspection](../source-artwork-inspection/SKILL.md) when the source asset has not been located. Use [2D Animation](../source-artwork-2d-animation/SKILL.md) after geometry is prepared.

## Workflow

1. **Preserve the input.** Keep an unchanged source file and provenance. Record `viewBox`, dimensions, coordinate units, layer/path order, fills, transforms, opacity, and clipping.
2. **Parse the actual format.** Use a standards-compliant SVG parser for general SVG. A narrow parser is acceptable only when the source grammar is verified; reject unsupported commands or elements rather than silently omitting them.
3. **Recover cells without redraw.** For pixel art, derive cell coordinates from source geometry, verify the grid origin and cell size, and preserve the original cell boundaries. Do not round arbitrary vector coordinates onto a grid unless the source supports that interpretation.
4. **Preserve visual invariants.** Retain palette values, registration, silhouette, fill rules, transforms, and draw order. Apply crisp-edge rendering only when the source is pixel-aligned and the target renderer supports it.
5. **Validate the prepared representation.** Compare source and parsed output at identical dimensions. Check bounds, cell count, path order, transparent regions, colors, and silhouette; use a lossless round-trip when conversion changes representation.
6. **Hand off geometry.** Provide the validated asset or geometry data with coordinate system, grid dimensions, palette, draw order, provenance, and any unsupported or inferred details.

## Boundaries

- Do not search websites or infer missing source assets; use [Source Artwork Inspection](../source-artwork-inspection/SKILL.md).
- Do not decode sprite atlases or packed frame streams; use [Sprite Frame Data](../source-artwork-sprite-frames/SKILL.md).
- Do not invent motion or 3D depth.

## Acceptance

- [ ] The unmodified source and its provenance remain available.
- [ ] Unsupported SVG constructs fail visibly instead of disappearing.
- [ ] Grid, palette, transforms, draw order, and registration are retained.
- [ ] Parsed or regenerated geometry matches the source at the intended display size.
- [ ] The downstream skill receives a documented coordinate system and geometry contract.
