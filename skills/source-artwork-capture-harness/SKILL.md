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

1. **Lock capture parameters.** Record viewport dimensions, device scale, background, crop, output size, color profile when available, and the source/output renderer versions.
2. **Lock the visual state.** Record interaction state, pose, frame index or timestamp, animation time, and reduced-motion setting. Freeze playback at the same state for source and output; do not rely on approximate manual timing.
3. **Stabilize rendering.** Wait for fonts, images, and layout to settle. Keep browser zoom, color settings, and transparent-background handling consistent. Repeat a capture to detect nondeterministic output.
4. **Render the control.** When renderers differ, pass unchanged source artwork through the output rendering path using the same capture parameters. Preserve this control separately; never retouch or normalize it after capture.
5. **Generate comparison material.** Produce source/output/control captures, a contact sheet, and overlays or difference images as appropriate. Keep original captures unchanged and identify each file with its state and parameters.
6. **Report capture limitations.** Record unsupported state controls, missing profile data, timing uncertainty, renderer differences, and any capture that could not be repeated. Let the battery mark affected metrics `not_testable` when the evidence is insufficient.

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

- [ ] Source, output, and control share the same capture parameters.
- [ ] Pose, state, and animation time are reproducible.
- [ ] Repeated captures expose or rule out unstable rendering.
- [ ] Originals remain unmodified and comparison artifacts are traceable.
- [ ] Limitations are explicit; no metric or verdict is invented.
