---
name: source-artwork-fidelity-battery
description: Independently audit 2D and 3D artwork reconstructions against source evidence using declared modes, measurable tolerances, and a portable proof packet.
---

# Source Artwork Fidelity Battery

Audit a completed source-artwork reconstruction independently from the workflow that produced it. Turn visual-fidelity claims into measured, evidence-backed pass/fail results. This skill is provider-, host-, OS-, renderer-, and tool-agnostic; use any available measurement method and never invent a measurement when a tool is unavailable. Use the [Capture Harness](../source-artwork-capture-harness/SKILL.md) for reproducible source/output/control captures.

## Select the Comparison Mode

Declare one comparison mode and one motion basis before scoring. A mode limits what can legitimately be claimed.

- **`source-frame-exact`:** Original vector/frame/model data exists. Compare the reconstruction against the same source pose and timing; compare source values directly when the render path is identical.
- **`source-observed`:** Only rendered behavior is available. Compare captures taken at matched states and record capture timing and uncertainty.
- **`static-adaptation`:** Only a still source exists. Score still-art fidelity and the authored animation's continuity; original motion fidelity is `not_testable`.
- **`2d-derived-3d`:** 3D is inferred from 2D. Score the matched front-view silhouette and source-visible features. Record inferred thickness, back surfaces, and hidden anatomy in `claims.adapted_or_inferred`; they are not check IDs or `not_testable` measurements.

Never combine results from different modes into one unqualified “1:1” claim. Use `claims.recovered`, `claims.adapted_or_inferred`, and `claims.not_testable` for claim-level statements; `not_testable` on a measurement is a status, not a scope category.

## Required Check Scope

Before measuring, classify every known check ID exactly once as `required`, `supporting`, or `out_of_scope`. Start with the mode’s core set, add conditional checks when the stated condition applies, and record all three sets in `check_scope`; give each out-of-scope check an ID-keyed reason. Use `—` in the matrix for no unconditional out-of-scope IDs. A required check marked `not_testable` blocks an unqualified 1:1 verdict.

| Mode | Core required checks | Add as required when applicable | Out-of-scope check IDs |
|---|---|---|---|
| `source-frame-exact` | `silhouette_iou`, `bbox_edge_drift_ratio` | `unexpected_palette_pixels` for finite source palettes; `source_frame_count_and_timing_match` when reproducing a source sequence; `loop_seam_landmark_displacement_ratio` for a seamless-loop claim; `unintended_attachment_gap_px` for articulated parts; `renderer_error` when renderers differ | `—` |
| `source-observed` | `silhouette_iou`, `bbox_edge_drift_ratio` | `unexpected_palette_pixels` for finite source palettes; `loop_period_error_ratio` when the source period is measurable; seam/attachment checks for those claims; `renderer_error` when renderers differ | `source_frame_count_and_timing_match` |
| `static-adaptation` | `silhouette_iou`, `bbox_edge_drift_ratio` | `unexpected_palette_pixels` when preserving a finite source palette; seam/attachment checks for authored loop/joint claims; `renderer_error` when renderers differ | `loop_period_error_ratio`, `source_frame_count_and_timing_match` |
| `2d-derived-3d` | `silhouette_iou`, `bbox_edge_drift_ratio` at the matched front view | `unexpected_palette_pixels` when the output claims exact source palette (flat/unlit); `loop_period_error_ratio` when source motion is observable; seam/attachment checks for those claims; `renderer_error` when renderers differ | `source_frame_count_and_timing_match` |

Conditional checks that do not apply may be `supporting` or `out_of_scope`; explain the choice for every out-of-scope ID. When 3D lighting intentionally introduces colors, record that adaptation in `claims.adapted_or_inferred` and classify the palette check out of scope with a reason. `renderer_error` is out of scope when source and output use the same renderer. A check that supports a recovered claim must be required even if the mode’s core list does not name it.


## Run the Battery

1. **Lock the reference and output.** Record source/output identifiers and checksums when available. Match viewport, browser zoom, scale, crop, background, pose, interaction state, animation time, `color_profile`, and `reduced_motion`; fill the reference/output captures and the control capture when renderers differ. Record each capture’s `timing_uncertainty_ms`. If profile or preference cannot be matched, mark affected checks `not_testable`. See the Capture Harness for field definitions.
2. **Choose measurable regions.** Define the subject mask, expected palette, registration bounds, landmarks, moving joints, and source frames before comparing. Exclude only documented background or intentional empty regions.
3. **Measure silhouette.** Compute mask intersection-over-union: `IoU = intersection_area / union_area`. Pass when `IoU >= tolerances.silhouette_iou_min`.
4. **Measure registration.** Compute the largest absolute difference among the subject bounding-box edges, divided by the reference bounding-box diagonal. Pass when the ratio is `<= tolerances.bbox_edge_drift_ratio_max`; also report pixel displacement at delivered size.
5. **Measure color.** For finite pixel palettes or an exact-palette claim, count unexpected colors and pass when `unexpected_palette_pixels <= tolerances.unexpected_palette_pixels_max`. If shading is intentionally authored, record it as an adaptation and scope the palette check out with a reason. For cross-renderer antialiasing, use the unchanged-source control instead of loosening the palette rule.
6. **Measure motion by source evidence.** Record `motion_basis` as `source_frames`, `visually_observed`, `authored_adaptation`, or `static_only`. For exact source-frame playback, compare frame order/count and encoded timing exactly. For observable loops, calculate `loop_period_error_ratio = abs(output_period_ms - reference_period_ms) / reference_period_ms` and `capture_uncertainty_ratio = (reference_period_uncertainty_ms + output_period_uncertainty_ms) / reference_period_ms`; derive both uncertainty bounds from the timestamp bounds of the captures used and record the derivation. If the reference period is missing/nonpositive, either period uncertainty is unknown, or the uncertainty ratio exceeds `tolerances.observed_loop_period_error_ratio_max`, mark the period check `not_testable`; otherwise pass when `loop_period_error_ratio <= tolerances.observed_loop_period_error_ratio_max`. Never score authored motion as recovered motion.
7. **Check the loop and attachments.** Calculate `loop_seam_landmark_displacement_ratio = maximum_first_to_last_landmark_displacement_px / reference_subject_bbox_diagonal_px` in matched capture coordinates; pass when the ratio is `<= tolerances.loop_seam_landmark_displacement_ratio_max`. Measure unintended attachment gaps in reference-image pixels at the recorded reference scale and pass when the gap is `<= tolerances.unintended_attachment_gap_px_max`; a gap present in the reference or explicitly intentional is not a failure.
8. **Account for renderer error.** When renderers differ, run the unchanged source through the output rendering path as a control and record its capture/tool details. For each renderer-sensitive metric, record control/reconstruction errors against the same reference and in the same units; calculate `excess_error = max(0, reconstruction_error - source_control_error)`. The tolerance is a ceiling: `excess_error <= tolerance` passes. Set each metric-specific tolerance before evaluating the output and record its metric, value, units, and rationale in `tolerances.overrides_and_reasons`. Add one `renderer_error.per_metric` record per affected metric, with its own status, reason, and evidence. Without a valid control, mark affected metrics `not_testable`.
9. **Set the verdict.** First classify every known check ID exactly once and set `check_scope.complete` to `true`. Any explicitly required check that fails makes `verdict` `fail`, even if scope is incomplete; otherwise, incomplete scope or any required `not_testable` check makes it `not_testable`; only a complete scope with all required checks passing yields `pass`. For a required `renderer_error`, any failed required per-metric record fails the check; otherwise a required untestable record makes it `not_testable`. Set `verdict_scope` to the tested claim and view; a required `not_testable` check blocks an unqualified 1:1 claim.
10. **Save the proof packet.** Copy `fidelity-battery-template.json`; fill source/output identity and checksums, `mode`, `motion_basis`, all captures, `rendering`, `check_scope`, every in-scope measurement, `claims`, `evidence`, `verdict`, `verdict_scope`, and `limitations`. Populate claim arrays with recovered facts, authored/inferred details, and unsupported claims; use empty lists only when none apply. For exact sequence checks, link both frame tables in `evidence`. Record tolerance overrides and reasons, and attach source/output comparisons.


## Check and Tolerance Index

The JSON packet is the single source for numeric defaults and metric-specific renderer ceilings. The procedure uses these field names; record justified overrides and per-metric rationales in `tolerances.overrides_and_reasons`.

| Check ID | Tolerance / comparison | Scope |
|---|---|---|
| `silhouette_iou` | `tolerances.silhouette_iou_min` | Source-matched 2D; 2D-derived 3D front view |
| `bbox_edge_drift_ratio` | `tolerances.bbox_edge_drift_ratio_max` | Matched view |
| `unexpected_palette_pixels` | `tolerances.unexpected_palette_pixels_max` | Finite palette / exact-palette claim |
| `loop_period_error_ratio` | `tolerances.observed_loop_period_error_ratio_max` | Measurable source animation |
| `loop_seam_landmark_displacement_ratio` | `tolerances.loop_seam_landmark_displacement_ratio_max` | Seamless-loop claim |
| `unintended_attachment_gap_px` | `tolerances.unintended_attachment_gap_px_max` | Intended connected joints |
| `renderer_error` | Ceiling: `excess_error <= tolerance`; per-metric tolerance is in `measurements.renderer_error.per_metric[].tolerance` | Per renderer-sensitive metric, same units as its formula |
| `source_frame_count_and_timing_match` | Exact source order/count/duration against the recorded frame table; no numeric tolerance field | Recovered source-frame sequence |

The defaults are practical acceptance gates, not a claim that every renderer or artwork has identical measurement noise. Never tune a tolerance after seeing a failing comparison without recording why.


## Portability and Evidence

Use any browser, image tool, renderer, script, or manual measurement method available. Do not require a specific model provider, coding-agent host, language, editor, or runtime. Record tool/version only to make results reproducible. If a check cannot be measured, mark it `not_testable` with the reason and evidence gap; never substitute an estimate while presenting it as a measurement.

## Acceptance Checklist

- [ ] Comparison mode, motion basis, and check scope match the available evidence.
- [ ] `check_scope.complete` is true and every check ID is classified exactly once; each out-of-scope ID has a reason.
- [ ] Reference/output/control captures match viewport, zoom, scale, crop, background, color profile, reduced-motion setting, pose, interaction state, and time; timing uncertainty is recorded.
- [ ] Every in-scope leaf measurement has a value, units, applicable tolerance, status, reason, and evidence; renderer-sensitive metrics have one complete per-metric control record.
- [ ] Renderer-sensitive comparisons use a same-pipeline source control or are `not_testable`.
- [ ] Inferences, adaptations, and untestable details are explicitly identified.
- [ ] Failed or untestable required checks are not reported as a 1:1 pass.
- [ ] The completed proof packet and contact sheets are reproducible.
