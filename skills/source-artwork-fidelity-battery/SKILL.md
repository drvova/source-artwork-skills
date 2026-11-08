---
name: source-artwork-fidelity-battery
description: Independently audit 2D and 3D artwork reconstructions against source evidence using declared modes, measurable tolerances, and a portable proof packet.
---

# Source Artwork Fidelity Battery

Audit a completed source-artwork reconstruction independently from the workflow that produced it. Turn visual-fidelity claims into measured, evidence-backed pass/fail results. This skill is provider-, host-, OS-, renderer-, and tool-agnostic; use any available measurement method and never invent a measurement when a tool is unavailable. Use the [Capture Harness](../source-artwork-capture-harness/SKILL.md) for reproducible source/output/control captures.

## Select the Comparison Mode

Declare one comparison mode, the exact claim scope, and all required checks before scoring. Freeze them, every selected threshold, and its rationale in `pre_registration`; the referenced artifact must predate scoring and include a hash or immutable commit. A mode limits what can legitimately be claimed.

- **`source-frame-exact`:** Original vector/frame/model data exists. Compare the reconstruction against the same source pose and timing; compare source values directly when the render path is identical. A whole-object 3D claim also requires required source-value geometry checks covering every claimed structural property.
- **`source-observed`:** Only rendered behavior is available. Compare captures taken at matched states and record capture timing and uncertainty.
- **`static-adaptation`:** Only a still source exists. Score still-art fidelity and the authored animation's continuity; original motion fidelity is `not_testable`.
- **`2d-derived-3d`:** 3D is inferred from 2D. Score the matched front-view silhouette and only source-visible features with explicit required measurements. Record inferred thickness, back surfaces, and hidden anatomy in `claims.adapted_or_inferred`; they are not check IDs or `not_testable` measurements. Do not claim whole-object 3D equivalence from 2D evidence.

Never combine results from different modes into one unqualified “1:1” claim. Use `claims.recovered`, `claims.adapted_or_inferred`, and `claims.not_testable` for claim-level statements; `not_testable` on a measurement is a status, not a scope category. Set `claims.whole_object_3d_equivalence_claimed` only when the whole-object gate in Step 9 is satisfied.

## Required Check Scope

The built-in check IDs are those in the index; custom IDs are the `id` values in `measurements.geometry_checks`. Before measuring, classify every built-in and custom ID exactly once as `required`, `supporting`, or `out_of_scope`. The required set must include the selected mode’s core checks and every check needed to support a recovered claim. `check_scope.complete` is derived as true only when the three sets are disjoint, cover exactly the known IDs, include every core and claim-supporting check in `required`, and every out-of-scope ID has an ID-keyed reason. An empty required set is invalid and cannot pass. A required check marked `not_testable` blocks a 1:1 verdict.

| Mode | Core required checks | Add as required when applicable | Out-of-scope check IDs |
|---|---|---|---|
| `source-frame-exact` | `silhouette_iou`, `bbox_edge_drift_ratio` | `unexpected_palette_pixels` for finite source palettes; `source_frame_count_and_timing_match` when reproducing a source sequence; `loop_seam_landmark_displacement_ratio` for a seamless-loop claim; `unintended_attachment_gap_px` for articulated parts; `renderer_error` when renderers differ; required `geometry_checks[]` for every whole-object 3D property claimed | `—` |
| `source-observed` | `silhouette_iou`, `bbox_edge_drift_ratio` | `unexpected_palette_pixels` for finite source palettes; `loop_period_error_ratio` when the source period is measurable; seam/attachment checks for those claims; `renderer_error` when renderers differ | `source_frame_count_and_timing_match` |
| `static-adaptation` | `silhouette_iou`, `bbox_edge_drift_ratio` | `unexpected_palette_pixels` when preserving a finite source palette; seam/attachment checks for authored loop/joint claims; `renderer_error` when renderers differ | `loop_period_error_ratio`, `source_frame_count_and_timing_match` |
| `2d-derived-3d` | `silhouette_iou`, `bbox_edge_drift_ratio` at the matched front view | `unexpected_palette_pixels` when the output claims exact source palette (flat/unlit); `loop_period_error_ratio` when source motion is observable; seam/attachment checks for those claims; `renderer_error` when renderers differ | `source_frame_count_and_timing_match` |

`views[]` lists each tested camera/projection/pose and links paired full capture records; the primary pair links to `reference.capture` and `output.capture`. For 2D, use `projection: "2d"` and explicit `n/a` camera/pose values. Every `geometry_checks[]` record has `id`, `kind`, `scope`, `formula`, `source_value`, `output_value`, `tolerance`, `tolerance_direction`, `units`, `status`, `reason`, and `evidence`. `kind: "source_value"` means the reference value is directly extracted from the source artifact, not inferred from a render; for whole-object 3D claims, `reference.asset_type` must be `3d_model`. The `scope` must name exactly which geometry or structure the metric covers; one arbitrary property cannot stand in for whole-mesh equivalence. Use `<=`, `>=`, or `==` as the tolerance direction, with units that match the formula.
Conditional checks that do not apply may be `supporting` or `out_of_scope`; explain every out-of-scope ID. When 3D lighting intentionally introduces colors, record that adaptation in `claims.adapted_or_inferred` and classify the palette check out of scope with a reason. `renderer_error` is out of scope when source and output use the same renderer. A source-visible feature or recovered property must have a required measurement; do not infer a pass from an unmeasured appearance. A 2D-derived model cannot support a source-value 3D geometry check.


## Run the Battery

1. **Lock the reference and output.** Record source/output identifiers, checksums, and `asset_type` (`vector_art`, `raster_art`, `frame_sequence`, `vector_animation`, `3d_model`, `rendered_animation`, or `other`). Match viewport, browser zoom, output dimensions, device scale, background, crop, pose, interaction state, animation time, `color_profile`, and `reduced_motion`; the capture records and bounding-box measurement establish scale. Fill the reference/output captures and the control capture when renderers differ. Record each capture’s `timing_uncertainty_ms`. If profile or preference cannot be matched, mark affected checks `not_testable`.
2. **Choose measurable regions.** Define the subject mask, expected palette, registration bounds, landmarks, moving joints, and source frames before comparing. Exclude only documented background or intentional empty regions.
3. **Measure silhouette.** Compute mask intersection-over-union: `IoU = intersection_area / union_area`. Pass when `IoU >= tolerances.silhouette_iou_min`.
4. **Measure registration.** Compute the largest absolute difference among the subject bounding-box edges, divided by the reference bounding-box diagonal. Pass when the ratio is `<= tolerances.bbox_edge_drift_ratio_max`; also report pixel displacement at delivered size.
5. **Measure color.** For finite pixel palettes or an exact-palette claim, count unexpected colors and pass when `unexpected_palette_pixels <= tolerances.unexpected_palette_pixels_max`. If shading is intentionally authored, record it as an adaptation and scope the palette check out with a reason. For cross-renderer antialiasing, use the unchanged-source control instead of loosening the palette rule.
6. **Measure motion by source evidence.** Record `motion_basis` as `source_frames`, `visually_observed`, `authored_adaptation`, or `static_only`. For exact source-frame playback, compare frame order/count and encoded timing exactly. For observable loops, define the same cycle-boundary event and record every consecutive occurrence—at least two timestamps for both reference and output—in `period_observations`; each occurrence carries its timestamp and upper-bound timing uncertainty. For each side, `period_ms = (last_time_ms - first_time_ms) / cycle_count` and `period_uncertainty_ms = (first_uncertainty_ms + last_uncertainty_ms) / cycle_count`, where `cycle_count = events.length - 1`; the nominal `loop_period_error_ratio = abs(output_period_ms - reference_period_ms) / reference_period_ms`. Let `[Rlo, Rhi] = [reference_period_ms - reference_period_uncertainty_ms, reference_period_ms + reference_period_uncertainty_ms]` and `[Olo, Ohi] = [output_period_ms - output_period_uncertainty_ms, output_period_ms + output_period_uncertainty_ms]`. If intervals overlap, the error-ratio lower bound is zero. If `Olo > Rhi`, it is `(Olo - Rhi) / Rhi`; if `Ohi < Rlo`, it is `(Rlo - Ohi) / Rlo`. The upper bound is the maximum of `abs(O-R)/R` over the four interval endpoint pairs. If the boundary is ambiguous, a cycle is missed, observations are insufficient, or either lower period bound is nonpositive, mark `not_testable`. Otherwise fail when the error-ratio lower bound exceeds `tolerances.observed_loop_period_error_ratio_max`, pass when the upper bound is at or below it, and mark `not_testable` when the interval straddles the threshold. Never score authored motion as recovered motion.
7. **Check the loop and attachments.** Calculate `loop_seam_landmark_displacement_ratio = maximum_first_to_last_landmark_displacement_px / reference_subject_bbox_diagonal_px` in matched capture coordinates; pass when the ratio is `<= tolerances.loop_seam_landmark_displacement_ratio_max`. Measure unintended attachment gaps in pixels of the aligned reference capture at its recorded output dimensions and device scale; pass when the gap is `<= tolerances.unintended_attachment_gap_px_max`. A gap present in the reference or explicitly intentional is not a failure.
8. **Account for renderer error.** When renderers differ, run the unchanged source through the output rendering path as a control and set `rendering.control_render.available` true only for a valid, unmodified same-pipeline capture. For each affected metric, store the sole tolerance value in `renderer_error.per_metric[].tolerance` and its rationale in `tolerance_rationale`; compute `excess_error = max(0, reconstruction_error - source_control_error)` in the same units and pass when `excess_error <= tolerance`. Freeze these values before scoring. `tolerances.overrides_and_reasons` is an object keyed by built-in tolerance field name; values are rationale strings only, never duplicate thresholds. If `renderer_error` is required, every listed per-metric record is required; no records or no valid control makes `renderer_error.status` `not_testable`. Set `renderer_error.status` to `fail` if any record fails, `not_testable` if no records or any record is untestable, and `pass` only when every record passes.
9. **Set the verdict.** Derive `check_scope.complete` using the set rules above; never set it true by assertion alone. A failed required check makes `verdict` `fail`, even when another check is untestable. Otherwise, missing/invalid pre-registration, incomplete scope, or any required `not_testable` check makes the verdict `not_testable`; only a valid pre-registration, complete scope, and all required checks passing yields `pass`. For whole-object 3D equivalence, require `reference.asset_type: "3d_model"`, at least three non-coplanar tested views covering the claimed surfaces, and required `geometry_checks[]` records of `kind: "source_value"` that compare the original model against the output and cover the full claimed geometry; require a passing check for every additional claimed structural property. Set `verdict_scope` to the tested claims and views.
10. **Save the proof packet.** Copy `fidelity-battery-template.json`; fill `pre_registration`, source/output identity and checksums, `mode`, `motion_basis`, all captures and tested views, `rendering`, `check_scope`, every in-scope built-in and custom measurement, `claims`, `evidence`, `verdict`, `verdict_scope`, and `limitations`. Populate claim arrays with recovered facts, authored/inferred details, and unsupported claims; use empty lists only when none apply. For exact sequence checks, link both frame tables in `evidence`. Preserve the frozen plan and attach source/output comparisons.


## Check and Tolerance Index

The JSON packet is the single source for numeric defaults and metric-specific renderer ceilings. Built-in overrides store rationale strings keyed by tolerance field; renderer-specific values and rationales live together in each `renderer_error.per_metric` record.

| Check ID | Tolerance / comparison | Scope |
|---|---|---|
| `silhouette_iou` | `tolerances.silhouette_iou_min` | Source-matched 2D; 2D-derived 3D front view |
| `bbox_edge_drift_ratio` | `tolerances.bbox_edge_drift_ratio_max` | Matched view |
| `unexpected_palette_pixels` | `tolerances.unexpected_palette_pixels_max` | Finite palette / exact-palette claim |
| `loop_period_error_ratio` | Compare its uncertainty interval with `tolerances.observed_loop_period_error_ratio_max`: lower bound above limit fails; upper bound at/below limit passes; overlap is `not_testable` | Measurable source animation |
| `loop_seam_landmark_displacement_ratio` | `tolerances.loop_seam_landmark_displacement_ratio_max` | Seamless-loop claim |
| `unintended_attachment_gap_px` | `tolerances.unintended_attachment_gap_px_max` | Intended connected joints |
| `renderer_error` | Ceiling: `excess_error <= tolerance`; value and rationale are in each required `measurements.renderer_error.per_metric[]` record | Per renderer-sensitive metric, same units as its formula |
| `geometry_checks[]` (custom IDs) | Per-record formula, `tolerance`, and `tolerance_direction`; exact checks use `==` | Native 3D structural claims |
| `source_frame_count_and_timing_match` | Exact source order/count/duration against the recorded frame table; no numeric tolerance field | Recovered source-frame sequence |

The defaults are practical acceptance gates, not a claim that every renderer or artwork has identical measurement noise. Freeze the mode, scope, claims, and thresholds before scoring. If a threshold must change after a comparison, create a new versioned audit, retain the original packet/results, and do not overwrite an original failure.


## Portability and Evidence

Use any browser, image tool, renderer, script, or manual measurement method available. Do not require a specific model provider, coding-agent host, language, editor, or runtime. Record tool/version only to make results reproducible. If a check cannot be measured, mark it `not_testable` with the reason and evidence gap; never substitute an estimate while presenting it as a measurement.

## Acceptance Checklist

- [ ] `pre_registration` identifies a timestamped, hashed or immutable plan that predates scoring and contains the mode, scope, claims, selected thresholds, and threshold rationales.
- [ ] `check_scope.complete` is derived from a disjoint, exhaustive classification of built-in and custom check IDs; `required` contains the mode core and every claim-supporting check.
- [ ] Reference/output/control captures match viewport, browser zoom, output dimensions, device scale, crop, background, color profile, reduced-motion setting, pose, interaction state, and time; subject scale is measured by the bbox check and timing uncertainty is recorded.
- [ ] Every in-scope measurement has a value, units, applicable comparison rule/tolerance, status, reason, and evidence; renderer-sensitive metrics have a valid control and aggregate status.
- [ ] Period comparisons record every consecutive matching cycle boundary (at least two timestamps) per source/output and compare the resulting error-ratio interval to the threshold; a missed cycle or ambiguous boundary is `not_testable`.
- [ ] Any custom geometry check has an ID, kind, scope, formula, source/output values, tolerance/direction or exact rule, units, status, reason, and evidence.
- [ ] A whole-object 3D equivalence claim has passing required source-value geometry checks covering the full claim; view-only evidence is not enough.
- [ ] Inferences, adaptations, and untestable details are explicitly identified; failed or untestable required checks are not reported as a 1:1 pass.
- [ ] Renderer-sensitive comparisons use a same-pipeline source control or are `not_testable`.
- [ ] The completed proof packet and contact sheets are reproducible; presentation-only fixtures are marked incomplete and non-reproducible.
