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
- A 3D output needs deterministic animation and reproducible media.

Use [Source Artwork Inspection](../source-artwork-inspection/SKILL.md) and [SVG and Pixel-Cell Geometry](../source-artwork-svg-cells/SKILL.md) to prepare missing inputs. This skill owns modeling, rigging, 3D motion, and render composition—not website asset discovery or fidelity thresholds.

## Workflow

1. **Choose geometry from evidence.** Use original model/rig data when available. For flat, color-separated art, use layered extrusion; use a mesh or deforming rig when curved volume is visible or required.
2. **Preserve the visible source.** Keep front silhouette, palette, scale, orientation, and color boundaries. Avoid unnecessary remeshing or redraws.
3. **Label inferred depth.** From a 2D-only source, infer only the thickness, back surfaces, and hidden anatomy needed to build the scene. Record those choices as inferred; never call the result the original 3D model.
4. **Build connected parts.** Place pivots at observed attachment points. Overlap or deform joint roots so gaps do not appear at full articulation. Do not split parts using coordinate bands without checking the resulting silhouette.
5. **Design a deterministic cycle.** Make the main action readable, then add restrained secondary motion only when observed or explicitly identified as authored. Offset paired motions deliberately and close the loop cleanly.
6. **Compose for the destination.** Match camera, lighting, background, scale, and crop to the source context. Render a supported animated format and a still fallback when needed.
7. **Inspect and refine.** Compare representative frames at final size. Check silhouette, attachments, loop seam, maximum stroke, face, occlusion, z-fighting, clipping, and color boundaries.
8. **Make regeneration reproducible.** Keep source data, model/composition code, and exact generation commands together. Regenerate affected media and inspect the rendered target page.

## Deliverables

- Source-derived geometry or a documented, reproducible model extraction path.
- A 3D rig/composition with deterministic timing and connected joints.
- An explicit list of inferred depth, hidden surfaces, and authored motion.
- Rendered animation, still fallback where needed, and regeneration instructions.

Use the [Capture Harness](../source-artwork-capture-harness/SKILL.md) for matched renders/control images and the [Fidelity Battery](../source-artwork-fidelity-battery/SKILL.md) for measured claims.

## Acceptance

- [ ] Visible silhouette, palette, proportions, and orientation trace to source evidence.
- [ ] Inferred geometry is labeled; recovered model data is not fabricated.
- [ ] Moving parts remain attached through the full cycle.
- [ ] The loop closes and the delivery format suits the target surface.
- [ ] A clean regeneration reproduces the checked-in output.
