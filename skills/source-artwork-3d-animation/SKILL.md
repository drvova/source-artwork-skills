---
name: source-artwork-3d-animation
description: Build and animate source-derived 3D artwork from prepared reference geometry, choosing extrusion or mesh, connected joints, and clearly labeled inferred depth.
---

# Source Artwork 3D Reconstruction and Animation

## Purpose

Build a 3D representation from prepared source artwork, then animate it without losing the source's visible identity. Use recovered model data when it exists; treat depth inferred from a flat image as an adaptation, never as recovered geometry.

## Activate When

- Prepared 2D geometry needs a source-derived 3D version.
- Existing 3D artwork needs its silhouette, joints, or motion corrected against source evidence.
- A decoded sprite sequence is the available reference for a 3D shape or motion adaptation.
- A 3D output needs deterministic animation and reproducible media.

Use [Source Artwork Inspection](../source-artwork-inspection/SKILL.md) and [SVG and Pixel-Cell Geometry](../source-artwork-svg-cells/SKILL.md) to prepare vector/cell inputs. For decoded sequences, use [Sprite Frame Data](../source-artwork-sprite-frames/SKILL.md) and carry its frame order, registration, palette, cadence, provenance, and validation evidence into this workflow. This skill owns modeling, rigging, 3D motion, and render composition—not asset discovery or fidelity thresholds.

## Workflow

1. **Choose geometry from evidence.** Use original model/rig data when available. For flat, color-separated art, use layered extrusion; use a mesh or deforming rig when curved volume is visible or required.
2. **Select frame evidence.** For decoded sequences, record the source frame indexes and timestamps used to establish shape and visible motion. Preserve registration; preserve exact source palette with flat/unlit materials, or label lighting and shading color changes as authored. Frames do not establish hidden depth.
3. **Preserve the visible source.** Keep the front silhouette, scale, orientation, and color boundaries. Avoid unnecessary remeshing or redraws; any intentional visible-color change is an authored adaptation, not recovered source color.
4. **Label inferred depth.** From a 2D-only source, infer only the thickness, back surfaces, and hidden anatomy needed to build the scene. Record those choices as inferred; never call the result the original 3D model.
5. **Build connected parts.** Place pivots at observed attachment points. Overlap or deform joint roots so gaps do not appear at full articulation. Do not split parts using coordinate bands without checking the resulting silhouette.
6. **Design a deterministic cycle.** Make the main action readable, then add restrained secondary motion only when observed or explicitly identified as authored. Offset paired motions deliberately and close the loop cleanly.
7. **Compose for the destination.** Match camera, lighting, background, scale, and crop to the source context. Render a supported animated format and a still fallback when needed.
8. **Inspect and refine.** Compare representative frames at final size. Check silhouette, attachments, loop seam, maximum stroke, face, occlusion, z-fighting, clipping, and color boundaries.
9. **Make regeneration reproducible.** Keep source data, model/composition code, and exact generation commands together. Regenerate affected media and inspect the rendered target page.

## Deliverables

- Source-derived geometry or a documented, reproducible model extraction path.
- A 3D rig/composition with deterministic timing and connected joints.
- An explicit list of inferred depth, hidden surfaces, authored motion, and lighting/shading color changes.
- Rendered animation, still fallback where needed, and regeneration inputs/instructions. If source or generator inputs cannot be included, document what is missing and do not claim reproducibility.

Use the [Capture Harness](../source-artwork-capture-harness/SKILL.md) for matched renders/control images and the [Fidelity Battery](../source-artwork-fidelity-battery/SKILL.md) for measured claims.

## Acceptance

- [ ] Visible silhouette, proportions, and orientation trace to source; palette is preserved or lighting/shading changes are labeled as authored.
- [ ] Sprite-frame source indexes, registration, and timing are recorded when applicable.
- [ ] Inferred geometry is labeled; recovered model data is not fabricated.
- [ ] Moving parts remain attached through the full cycle; the loop closes cleanly.
- [ ] The delivery format and fallback suit the target surface.
- [ ] When regeneration inputs are included, a clean regeneration reproduces the checked-in output; otherwise the missing inputs are documented and reproducibility is not claimed.
