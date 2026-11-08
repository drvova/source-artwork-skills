---
name: source-artwork-capture-harness
description: Produce repeatable matched artwork captures and unchanged-source control renders at fixed viewport, scale, background, crop, pose, and animation time.
---

# Source Artwork Capture Harness

## Purpose

Create comparable source, output, and control images for visual review or a fidelity audit. This skill owns capture repeatability—not metric thresholds or pass/fail verdicts.

## Activate When

- Source and reconstruction captures must be compared at matched conditions.
- Animation must be frozen at a known pose or timestamp.
- Cross-renderer error needs an unchanged-source control render.

Use the [Fidelity Battery](../source-artwork-fidelity-battery/SKILL.md) to score captured evidence.

## Workflow

1. **Lock capture parameters.** Record viewport dimensions, device scale, output pixel dimensions, background, crop, `color_profile`, and source/output renderer versions. Record each capture's `timing_uncertainty_ms` as an upper bound in milliseconds.
2. **Lock the visual state.** Record a concise named `state` (for example, idle, hover, or focused), the interaction sequence, pose, frame index or timestamp, animation time, and `reduced_motion`. Match color profile and reduced-motion preference across source, output, and control; if either cannot be matched or measured, record the gap and mark affected checks `not_testable`. Freeze playback at the same state for source and output; do not rely on approximate manual timing.
3. **Record view geometry.** For each tested 3D view, record `view_id`, projection, camera pose, object pose, and paired source/output capture-record locations in `views[]`. Each linked capture record must include the complete parameters from this harness; camera, projection, pose, viewport, and device scale must match within the declared capture method.
4. **Stabilize rendering.** Wait for fonts, images, and layout to settle. Keep browser zoom, color settings, and transparent-background handling consistent. Repeat a capture to detect nondeterministic output.
5. **Render the control.** When renderers differ, pass unchanged source artwork through the output rendering path using the same capture parameters. Preserve this control separately; never retouch or normalize it after capture.
6. **Generate comparison material.** Produce source/output/control captures, a contact sheet, and overlays or difference images as appropriate. Keep original captures unchanged and identify each file with its state and parameters.
7. **Report capture limitations.** Record unsupported state controls, missing profile data, timestamp uncertainty bounds, renderer differences, and any capture that could not be repeated. For a period comparison, define one repeatable cycle-boundary event and record every consecutive occurrence (at least two timestamps) for both source and output, with each timestamp's uncertainty; if a cycle is missed or the boundary is ambiguous, let the Fidelity Battery mark the period metric `not_testable`.

## Boundaries

- Do not choose tolerances, calculate a verdict, or loosen a failing threshold; use the [Fidelity Battery](../source-artwork-fidelity-battery/SKILL.md).
- Do not author or alter source geometry to make captures appear closer.
- Do not claim pixel identity across renderers without a valid control comparison.

## Deliverables

- Matched, unmodified source and output captures.
- An unchanged-source control render when renderer paths differ.
- Capture parameters, state identifiers, and renderer details.
- Contact sheets, overlays, or difference images with provenance.

## Acceptance

- [ ] Source, output, and control share capture parameters, including `color_profile` and `reduced_motion`; any unavoidable mismatch is recorded and affected checks are `not_testable`.
- [ ] Pose, state, animation time, and timestamp uncertainty are recorded for reproducible comparisons.
- [ ] Repeated captures expose or rule out unstable rendering.
- [ ] Originals remain unmodified and comparison artifacts are traceable.
- [ ] Limitations are explicit; no metric or verdict is invented.
