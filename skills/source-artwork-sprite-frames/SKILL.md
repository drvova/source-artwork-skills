---
name: source-artwork-sprite-frames
description: Decode, validate, and round-trip sprite sheets, image sequences, and packed frame grids while preserving registration, frame order, palette, and timing.
---

# Sprite Frame Data

## Purpose

Turn source frame data into a validated, documented sequence for downstream animation work. Decode the actual source format; do not guess a schema from a visual preview.

## Activate When

- Artwork is stored as a sprite sheet, frame sequence, packed cell grid, or encoded frame stream.
- Frame registration, order, dimensions, timing, or palette must be recovered.
- A custom decoder or data layout needs validation before editing.

Use [Source Artwork Inspection](../source-artwork-inspection/SKILL.md) to locate the source and observe playback. Use [2D Animation](../source-artwork-2d-animation/SKILL.md) to author or implement motion after decoding.

## Workflow

1. **Preserve the source bytes.** Record provenance, checksum when available, dimensions, metadata, and the runtime path that consumes the data.
2. **Read the decoder before editing data.** Trace frame boundaries, dimensions, cell size, palette indices, alpha/size flags, compression, registration, and timing fields from actual code or a verified format specification.
3. **Define the decoded frame contract.** Record frame index, source offset or rectangle, canvas size, origin/registration point, duration, palette, and loop semantics. Keep unknown fields explicitly unknown.
4. **Validate bounds and values.** Reject truncated frames, out-of-range indices, invalid dimensions, unsupported flags, and overflow. Do not silently drop cells, clamp malformed values, or infer missing frames.
5. **Round-trip a sample.** Decode and re-encode representative frames, then compare dimensions, pixel values, transparency, registration, and bytes where the format permits byte-identical round-tripping. Explain any canonicalization that prevents byte identity.
6. **Verify the sequence.** Render a contact sheet and inspect frame order, palette, alignment, duplicates, timing, and loop closure against the source behavior.
7. **Hand off prepared frames.** Supply decoded frame data plus its schema, frame table, registration, palette, cadence, loop behavior, provenance, and validation evidence.

## Boundaries

- Do not locate website assets or infer runtime behavior; use [Source Artwork Inspection](../source-artwork-inspection/SKILL.md).
- Do not rig parts or implement playback; use [2D Animation](../source-artwork-2d-animation/SKILL.md).
- Do not assign a pass/fail fidelity verdict.

## Acceptance

- [ ] The decoder and data schema are understood before edits.
- [ ] Frame bounds, flags, indices, registration, and timing validate.
- [ ] A representative decode/re-encode round-trip has been compared.
- [ ] The output preserves source frame order and visual content.
- [ ] Downstream animation work receives a complete frame contract.
