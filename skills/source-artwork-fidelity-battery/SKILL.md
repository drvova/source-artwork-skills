---
name: source-artwork-fidelity-battery
description: Independently audit 2D and 3D artwork reconstructions against source evidence using declared modes, measurable tolerances, and a portable proof packet.
---

# Source Artwork Fidelity Battery

Audit a completed source-artwork reconstruction independently from the workflow that produced it. Turn visual-fidelity claims into measured, evidence-backed pass/fail results. This skill is provider-, host-, OS-, renderer-, and tool-agnostic; use any available measurement method and never invent a measurement when a tool is unavailable.

## Select the Comparison Mode

Declare one comparison mode and one motion basis before scoring. A mode limits what can legitimately be claimed.

- **`source-frame-exact`:** Original vector/frame/model data exists. Compare the reconstruction against the same source pose and timing; compare source values directly when the render path is identical.
- **`source-observed`:** Only rendered behavior is available. Compare captures taken at matched states and record capture timing and uncertainty.
- **`static-adaptation`:** Only a still source exists. Score still-art fidelity and the authored animation's continuity; original motion fidelity is `not_testable`.
- **`2d-derived-3d`:** 3D is inferred from 2D. Score the matched front-view silhouette, registration, and visible colors; mark hidden surfaces and unseen depth `not_testable` and list them as inferred.

Never combine results from different modes into one unqualified “1:1” claim. Keep recovered facts, authored adaptations, and untestable details separate.

## Run the Battery

1. **Lock the reference.** Record the source URL/path and checksum when available. Capture reference and output at matched viewport, scale, crop, background, pose, and animation time. Record the exact state and capture method.
2. **Choose measurable regions.** Define the subject mask, expected palette, registration bounds, landmarks, moving joints, and source frames before comparing. Exclude only documented background or intentional empty regions.
3. **Measure silhouette.** Compute mask intersection-over-union: `IoU = intersection_area / union_area`. Default pass: `IoU >= 0.98` for source-matched 2D art and a 2D-derived 3D front view.
4. **Measure registration.** Compute the largest absolute difference among the subject bounding-box edges, divided by the reference bounding-box diagonal. Default pass: `<= 0.01` (1%). Also report pixel displacement at the delivered size.
5. **Measure color.** For pixel art or preserved source color values, require exact palette entries and `0` unexpected-color pixels. For cross-renderer antialiasing, measure against a source control render and report the renderer-adjusted result instead of silently loosening the palette rule.
6. **Measure motion by source evidence.** Record `motion_basis` as `source_frames`, `visually_observed`, `authored_adaptation`, or `static_only`. For `source-frame-exact` with source frames, frame order/count and encoded timing must match exactly. For `source-observed`, loop-period error must be at most `5%` of the measured reference period unless capture uncertainty is larger; then mark timing `not_testable`. For `static-adaptation`, source-motion equivalence is `not_testable`. For `2d-derived-3d`, score timing and visible pose correspondence only when source frames or observable source motion exist; otherwise mark source-motion equivalence `not_testable`. Never score authored motion as recovered motion.
7. **Check the loop and attachments.** For a seamless loop, maximum first-to-last landmark displacement defaults to `<= 0.01` of the subject bounding-box diagonal. Inspect the largest articulation at delivered size; any exposed gap over `1` source pixel at an intended attachment fails unless the gap is present in the reference or explicitly intentional.
8. **Account for renderer error.** When renderers differ, run the unchanged source through the output rendering path as a control. For each metric, record control and reconstruction errors against the same reference, in the same units; calculate `excess_error = max(0, reconstruction_error - source_control_error)`. Apply the default tolerance to excess error. Without a valid control, mark renderer-sensitive metrics `not_testable`; do not label them pixel-exact.
9. **Set the verdict.** Each applicable check is `pass`, `fail`, or `not_testable`; record the claim scope in `verdict_scope`. Any failed required check makes the battery `fail`. Any required `not_testable` check prevents an unqualified 1:1 verdict. A passing adaptation can claim source-faithful appearance at the tested view—not recovered motion, hidden geometry, or original authorship.
10. **Save the proof packet.** Copy `fidelity-battery-template.json` from this skill folder and fill every field. Attach source/output contact sheets and comparison images where available. Record tolerance overrides and their evidence-based reasons.

## Default Tolerance Summary

| Check | Default pass threshold | Scope |
|---|---:|---|
| Silhouette IoU | `>= 0.98` | Source-matched 2D; 2D-derived 3D front view |
| Bounding-box edge drift | `<= 0.01` of reference diagonal | Matched view |
| Unexpected palette pixels | `0` | Pixel art / source palette preserved |
| Observed loop-period error | `<= 0.05` | Measurable source animation |
| Loop seam landmark displacement | `<= 0.01` of reference diagonal | Seamless-loop claim |
| Unintended attachment gap | `<= 1` source pixel | Intended connected joints |
| Frame count and timing | Exact source order/count/duration | Recovered source frames |

These defaults are practical acceptance gates, not a claim that every renderer or artwork has identical measurement noise. Declare justified overrides in the packet before interpreting results; never tune tolerances after seeing a failing comparison without recording why.

## Portability and Evidence

Use any browser, image tool, renderer, script, or manual measurement method available. Do not require a specific model provider, coding-agent host, language, editor, or runtime. Record tool/version only to make results reproducible. If a check cannot be measured, mark it `not_testable` with the reason and evidence gap; never substitute an estimate while presenting it as a measurement.

## Acceptance Checklist

- [ ] Comparison mode and motion basis match the available evidence.
- [ ] Reference and output use matched states, scale, crop, background, and timing.
- [ ] Every applicable metric has a value, tolerance, status, and evidence reference.
- [ ] Renderer-sensitive comparisons use a same-pipeline source control or are `not_testable`.
- [ ] Inferences, adaptations, and untestable details are explicitly identified.
- [ ] Failed or untestable required checks are not reported as a 1:1 pass.
- [ ] The completed proof packet and contact sheets are reproducible.
