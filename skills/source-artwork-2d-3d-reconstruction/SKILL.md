---
name: source-artwork-2d-3d-reconstruction
description: Route end-to-end source-artwork work across inspection, geometry recovery, 2D or 3D authoring, matched capture, and independent fidelity review. Use focused skills directly for a single stage.
---

# Source Artwork Reconstruction Router

## Purpose

Choose the requested output tracks, assemble only the specialist skills needed, and keep their handoffs explicit. This skill routes work; it does not duplicate the procedures owned by its leaves.

## Activate When

- The request spans multiple source-artwork stages or both 2D and 3D.
- The correct specialist is unclear.
- A source-faithful result needs a complete inspection-to-validation workflow.

For one clearly scoped stage, use its focused skill directly.

## Route

| Task | Specialist |
|---|---|
| Find the real source, inspect behavior, record evidence | [Source Artwork Inspection](../source-artwork-inspection/SKILL.md) |
| Preserve or inspect SVG paths and pixel-cell geometry | [SVG and Pixel-Cell Geometry](../source-artwork-svg-cells/SKILL.md) |
| Decode or validate sprite sheets and packed frame data | [Sprite Frame Data](../source-artwork-sprite-frames/SKILL.md) |
| Author 2D motion and playback from prepared source data | [2D Animation](../source-artwork-2d-animation/SKILL.md) |
| Build a source-derived 3D model and motion | [3D Animation](../source-artwork-3d-animation/SKILL.md) |
| Produce matched captures and renderer control images | [Capture Harness](../source-artwork-capture-harness/SKILL.md) |
| Score a completed result independently | [Fidelity Battery](../source-artwork-fidelity-battery/SKILL.md) |

## Common Routes

| Goal | Ordered handoff |
|---|---|
| Reproduce existing 2D animation | Inspection → SVG/cells or sprite frames → 2D animation → capture → fidelity battery |
| Animate a still in 2D | Inspection → SVG/cells → 2D animation → capture → fidelity battery |
| Derive 3D from 2D art | Inspection → SVG/cells or sprite frames → 3D animation → capture → fidelity battery |
| Audit an existing reconstruction | Capture, if matched evidence is missing → fidelity battery |

Skip any stage whose verified artifact already exists. Do not re-run work merely to follow the table.

## Fidelity Contract

Declare one comparison mode and one motion basis before scoring. The [Fidelity Battery](../source-artwork-fidelity-battery/SKILL.md) is the sole owner of mode definitions, check scope, measurement formulas, and numeric tolerances.

Use its required-check matrix and packet to scope evidence. An unqualified 1:1 claim requires every required check to be measurable and pass.

Never present authored motion, inferred geometry, or unobserved details as recovered source data.

## Handoff Contract

- **Evidence record:** source location, provenance, capture conditions, confirmed facts, inferences, and unknowns.
- **Preserved source:** unmodified input plus enough provenance to identify it.
- **Prepared data:** validated geometry/cells or decoded frames, with palette, registration, and timing where applicable.
- **Comparison set:** matched source/output captures and any unchanged-source control render.
- **Proof packet:** completed Fidelity Battery record with evidence references and a scoped verdict.

Pass artifacts forward; do not copy another skill's procedure into this router. If a required stage or capability is unavailable, record the evidence gap and let the battery mark affected checks `not_testable`.

## Portability

The skill family is independent of model provider, agent host, operating system, editor, renderer, language, and framework. Use project-supported tools, inspect their current APIs, and keep tool-specific commands in project scripts. A missing browser, asset, or capture capability is an evidence limit—not permission to invent results.

## Completion

- Every requested output track has an explicit owner and deliverable.
- Source facts, authored adaptations, and inferred details remain distinct.
- Reproducible assets and relevant fallback media are handed off.
- Validation claims are scoped to the available evidence.
