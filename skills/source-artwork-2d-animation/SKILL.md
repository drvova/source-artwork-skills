---
name: source-artwork-2d-animation
description: Author 2D motion and playback from inspected, prepared artwork, including attached part rigs, frame cadence, interaction, and reduced-motion output.
---

# Source Artwork 2D Animation

## Purpose

Turn prepared source geometry or decoded frames into deterministic 2D motion. Preserve source identity and distinguish recovered motion from animation newly authored from a still.

## Activate When

- Source assets or decoded geometry/frame data are already available.
- The task is to rig, animate, play back, export, or refine 2D artwork.
- A static mark needs an explicitly identified motion adaptation.

Use [Source Artwork Inspection](../source-artwork-inspection/SKILL.md) to locate and characterize missing source data. Use [SVG and Pixel-Cell Geometry](../source-artwork-svg-cells/SKILL.md) or [Sprite Frame Data](../source-artwork-sprite-frames/SKILL.md) to prepare source representations. This skill does not reverse-engineer a site or decode custom frame formats.

## Workflow

1. **Accept prepared inputs.** Confirm source provenance, canvas/viewBox, palette, registration, draw order, and whether motion is recovered or authored.
2. **Plan motion.** Define the action, key poses, contact poses, cadence, loop point, and interaction states before editing.
3. **Rig articulated geometry where appropriate.** Group vector or pixel-cell source parts at visible boundaries; place pivots on attachment points and preserve overlap/draw order through the full stroke. Keep pixel edits grid-aligned. For an existing frame sequence, preserve its registration and play the decoded frames instead of forcing a part rig.
4. **Implement deterministic playback.** Advance using elapsed time or the host's stable frame scheduler. Preserve observed cadence and loop semantics; prevent duplicate animation loops.
5. **Wire supported interaction.** Handle pointer, keyboard focus, leave/blur, and stop behavior deliberately. Respect reduced-motion preferences.
6. **Export for the target.** Use a format the destination supports. Supply a static fallback and useful alt text where the surface supports them.
7. **Verify the delivered result.** Inspect every key pose, articulation extreme, loop seam, final display size, crop, palette, and reduced-motion path. Rebuild and inspect the actual page or README.

## Deliverables

- 2D animation code or source frame sequence.
- Clear recovered-motion versus authored-adaptation labeling.
- Target-ready animated output and static fallback where applicable.
- Regeneration instructions and inspected key-pose evidence.

For measured pass/fail claims, use the independent [Fidelity Battery](../source-artwork-fidelity-battery/SKILL.md). Use the [Capture Harness](../source-artwork-capture-harness/SKILL.md) when comparison states or control renders must be reproduced.

## Acceptance

- [ ] Source geometry, grid, palette, and registration remain intact.
- [ ] Moving parts stay connected with intentional overlap and draw order.
- [ ] Timing and loop behavior are deterministic and evidence-based.
- [ ] Interaction and reduced-motion behavior work on the target surface.
- [ ] A clean regeneration reproduces the delivered output.
