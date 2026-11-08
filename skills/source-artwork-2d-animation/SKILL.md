---
name: source-artwork-2d-animation
description: Reverse-engineer existing pixel-art or vector animation and create faithful, accessible 2D frame animation for websites, documentation, and product assets.
---

# Source Artwork to 2D Animation

## Purpose

Inspect how an existing 2D mascot or mark is represented and animated, then reproduce or extend it without guessing at its source or authoring tool. Preserve the original silhouette, palette, grid, timing, and interaction where the evidence supports them.

## Portability Contract

This workflow is independent of model provider, agent host, operating system, authoring application, renderer, and framework. Use equivalent tools exposed by the project or host; do not require React, a particular browser automation service, editor, or encoder. Keep tool-specific commands in project scripts. If a capability is unavailable, use supplied assets or record the evidence limit rather than inventing results.

## Source and Motion Fidelity

- **Recovered animation:** When original frame, path, or rig data exists, preserve its geometry, timing, registration, and behavior.
- **New animation from a still:** Preserve the source artwork; label invented poses, timing, and secondary motion as a new adaptation.
- **Unknown authoring tool:** Report it as unknown unless source files, metadata, or an explicit attribution prove it.

## Activate When

- A website or app contains an animated logo, sprite, mascot, or pixel-art mark.
- A user wants to reproduce, improve, or document frame-based SVG/canvas animation.
- The original authoring tool or custom sprite format is unknown and needs investigation.
- A README or other static surface needs an animated preview and reduced-motion fallback.

## Required Workflow

1. **Inspect the rendered result and source.** Observe idle, hover, focus, pointer, and reduced-motion states. Locate SVG markup, image sequences, sprite sheets, canvas drawing, source files, and relevant scripts or styles. Record viewBox/canvas dimensions, grid size, palette, crop, and responsive behavior.
2. **Separate authoring from playback.** Trace the runtime: interaction handlers, frame data, decoder, frame rate, loop/stop behavior, and renderer. Search source maps, repository files, metadata, credits, and dependencies for an explicit authoring-tool attribution. A packed sprite format or SVG output does not identify the editor that created it. If no evidence names the tool, say it is unknown; do not guess.
3. **Preserve the original asset.** Keep a source copy and note its URL or repository path. Preserve layer order, palette values, frame registration, coordinate units, and viewBox. Do not redraw from a screenshot when original vector or frame data is available.
4. **Understand custom encodings before editing.** Read the decoder and data schema first. Identify frame boundaries, cell dimensions, color indices, size/alpha flags, and compression. Validate bounds and supported values; decode and re-encode a sample, then compare the result with the original. Reject malformed or unsupported data rather than silently dropping cells.
5. **Plan the animation as drawings.** Define the action, key/contact poses, timing, and loop point. Keep canvas bounds and character registration stable. For articulated vector or pixel-cell art, group source geometry by visible parts and place pivots at observed joints; preserve draw order and overlap roots so parts stay attached through the full stroke. Avoid arbitrary coordinate cuts. Use distinct key poses and deliberate in-betweens; preserve a readable silhouette and consistent pixel scale. For pixel art, align edits to the source grid and use the original palette unless a change is intentional.
6. **Choose a representation that fits.** Keep vector paths when crisp scalable geometry matters; use raster frames or a sprite sheet when that matches the source; use compact cell data only when the project needs it. Avoid introducing a new framework or custom codec if the current project already has a sound path.
7. **Implement predictable playback.** Advance by elapsed time rather than assuming display refresh rate equals animation frame rate. Preserve the observed frame cadence and loop semantics when reproducing a reference. Wire interaction to pointer and keyboard focus, handle pointer leave/blur deliberately, and respect `prefers-reduced-motion`. Avoid starting duplicate animation loops.
8. **Export for the destination.** Choose a format the target surface actually supports. When animated SVG or CSS is unsupported, render frames to an appropriate animated image or video format. Provide a static reduced-motion fallback where the host supports motion preferences; otherwise place a still beside the animation. Keep crop, scale, background, and alt text consistent with the destination.
9. **Verify the result.** Inspect every key pose and the loop boundary at final display size. Check pixel registration, silhouette, palette, frame duration, accidental flicker, clipping, file size, hover/focus behavior, and reduced-motion output. Compare decoded output to the source, not only to your own preview.
10. **Make it reproducible.** Keep the original asset, decoder/authoring data, playback code, generation scripts, and regeneration command together. Rebuild every affected output after a source or timing change, then inspect the rendered page or README.

## Tool Selection

Use the tools and libraries already supported by the project, and inspect their actual documentation before relying on a format or API. Editors, browser inspectors, SVG renderers, encoders, and frame generators are interchangeable implementation choices—not dependencies required by this skill. Do not assume network, browser, shell, or filesystem access; use available evidence and state any resulting limitation.

## Deliverables

- Verified source asset and provenance, or a clear statement of what could not be recovered.
- Source-faithful animation or an explicitly identified adaptation from a static source.
- Frame, vector, or sprite data with a documented format when custom encoded.
- Runtime playback that matches the intended cadence and interaction.
- Target-ready animation and a still fallback where needed.
- Reproducible generation instructions and inspected frame/loop evidence.

For a separate quantitative pass/fail review, pair this workflow with the standalone Source Artwork Fidelity Battery. This skill remains usable without it.

## Acceptance Checklist

- [ ] Source animation is preserved, or newly authored motion is identified as an adaptation.
- [ ] Authoring-tool claims are evidenced or explicitly marked unknown.
- [ ] Source geometry, palette, dimensions, and frame registration are preserved.
- [ ] Articulated parts stay attached and retain draw order through their full stroke.
- [ ] Custom frame data decodes and validates without silent losses.
- [ ] Playback timing and loop/stop behavior are intentional and repeatable.
- [ ] Pointer, keyboard focus, and reduced-motion behavior are handled where supported.
- [ ] The destination displays the animation and fallback correctly at final size.
- [ ] A clean regeneration reproduces the delivered assets.
