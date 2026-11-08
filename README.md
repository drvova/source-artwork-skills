# Source Artwork Skills

Portable, provider-agnostic agent skills for inspecting real artwork and recreating its 2D animation and 3D form faithfully.

## Skill map

### Router

- **[Source Artwork Reconstruction Router](skills/source-artwork-2d-3d-reconstruction/SKILL.md)** — selects the tracks and sequences focused skills for end-to-end work.

### Focused skills

- [Source Artwork Inspection](skills/source-artwork-inspection/SKILL.md) — locate assets, observe behavior, and separate evidence from inference.
- [SVG and Pixel-Cell Geometry](skills/source-artwork-svg-cells/SKILL.md) — preserve vector paths, grid-aligned cells, palette, and registration.
- [Sprite Frame Data](skills/source-artwork-sprite-frames/SKILL.md) — decode, validate, and round-trip frame sheets and packed formats.
- [2D Animation](skills/source-artwork-2d-animation/SKILL.md) — rig prepared geometry or frames, author motion, and implement playback.
- [3D Animation](skills/source-artwork-3d-animation/SKILL.md) — build source-derived geometry, joints, deterministic motion, and renders.
- [Capture Harness](skills/source-artwork-capture-harness/SKILL.md) — reproduce matched captures and unchanged-source control renders.
- [Fidelity Battery](skills/source-artwork-fidelity-battery/SKILL.md) — independently score a result and record a scoped proof packet.

Each stage owns one concern. The router defines sequence and handoff artifacts; focused skills own implementation procedure. Use existing verified artifacts instead of repeating earlier stages.

## Worked example: 2D and 3D axolotl animations

<table>
  <tr>
    <th>2D animation (source-cell rig)</th>
    <th>3D reconstruction (animated)</th>
  </tr>
  <tr>
    <td>
      <picture>
        <source srcset="examples/axolotl-2d-source.svg" media="(prefers-reduced-motion: reduce)">
        <img src="examples/axolotl-2d-animation.svg" alt="The pixel-art axolotl swims in place in 2D, with its gills paddling, legs sweeping, and eyes blinking.">
      </picture>
    </td>
    <td>
      <picture>
        <source srcset="examples/axolotl-3d-demo-still.png" media="(prefers-reduced-motion: reduce)">
        <img src="examples/axolotl-3d-demo.gif" alt="A 3D pixel-art axolotl swimming in place, with gills and limbs moving.">
      </picture>
    </td>
  </tr>
</table>

Both versions start from the same exact source cells. The left is an authored 2D animation; the right extrudes those cells into a source-derived 3D animation. Static source art is used when reduced motion is preferred.

1. **Inspect the input.** The [inspection skill](skills/source-artwork-inspection/SKILL.md) records the source asset, rendered states, and confirmed facts separately from inference. The mark was a 312 × 266 SVG with seven colored paths made from 253 rectangular pixel-art cells. The website's animation was separate from the static logo.
2. **Trace the original motion.** The [sprite-frame skill](skills/source-artwork-sprite-frames/SKILL.md) covers decoder-first analysis and frame validation. The inspected site bundle contained 24 frames on a 39 × 28 cell grid, packed as base64 and decoded to SVG at 12 fps. Hover and keyboard focus drove playback. The bundle exposed the playback format, but not the artist's drawing tool.
3. **Animate in 2D.** Keep the source cells and colors intact; rig the gills and limbs at their attachment pivots, then add body sway and a blink. This demo authors a new 2D swim cycle from the source geometry rather than claiming to replay the site's exact 24 frames.
4. **Build the 3D version.** Extrude the same colored cells into shallow layers and group body, gills, eyes, and limbs. Keep pivots at real attachment points; add body sway, breathing, limb strokes, gill motion, and a blink without losing the source silhouette.
5. **Render and check.** The [capture harness](skills/source-artwork-capture-harness/SKILL.md) handles matched comparison inputs; the [fidelity battery](skills/source-artwork-fidelity-battery/SKILL.md) owns the thresholds and verdict. Inspect key poses for silhouette drift, detached joints, clipping, and loop seams. Both animations include static reduced-motion fallbacks.

**Fidelity boundary:** the website's own animation was 2D, and its frame data was inspected. The 2D animation shown here is a new source-cell rig, not a frame-for-frame export of that player. The 3D GIF is a reconstruction from the same visible 2D artwork—not an original 3D model recovered from the site—so depth and hidden surfaces are inferred. The MIT license covers the skills and documentation; the axolotl artwork remains with its respective rights holder.


## More animated 2D pixel-cell examples

<table>
  <tr>
    <th>Butterfly — hinged wing cells</th>
    <th>Jellyfish — pulsing bell and tentacle cells</th>
  </tr>
  <tr>
    <td>
      <picture>
        <source srcset="examples/butterfly-animation-still.png" media="(prefers-reduced-motion: reduce)">
        <img src="examples/butterfly-animation.gif" width="320" height="256" alt="A pixel-cell butterfly flaps its wings and hovers over a meadow.">
      </picture>
    </td>
    <td>
      <picture>
        <source srcset="examples/jellyfish-animation-still.png" media="(prefers-reduced-motion: reduce)">
        <img src="examples/jellyfish-animation.gif" width="320" height="256" alt="A pixel-cell jellyfish pulses its bell and sways four tentacles underwater.">
      </picture>
    </td>
  </tr>
</table>

Both are original, MIT-licensed demonstrations built from explicit 8 × 8 SVG cells—not animations recovered from a website. The butterfly rigs its two wings around their attachment pivots; the jellyfish coordinates a bell pulse with four separate tentacle sweeps. The editable [butterfly SVG](examples/butterfly-animation.svg) and [jellyfish SVG](examples/jellyfish-animation.svg) are the animation sources; their GIFs are rendered previews that play reliably in GitHub Markdown, and the static PNGs are reduced-motion fallbacks.

## What “1:1” means here

When the original 2D paths, frames, or model are available, the skills direct the agent to preserve them and verify against matched captures. If only a flat 2D reference exists, hidden 3D depth and surfaces cannot be recovered exactly; the unified skill requires those parts to be identified as inferred rather than presented as the original model.

## Quantified Fidelity Check

Use the [Fidelity Battery](skills/source-artwork-fidelity-battery/SKILL.md) after any reconstruction when you need a reviewable pass/fail result. It separates source-frame-exact, source-observed, static-adaptation, and 2D-derived-3D claims so invented motion or hidden geometry is never scored as recovered. Default checks include silhouette IoU `>= 0.98`, bounding-box drift `<= 1%`, and loop-seam displacement `<= 1%` of the subject bounds. Cross-renderer comparisons use an unchanged-source control render, which the [Capture Harness](skills/source-artwork-capture-harness/SKILL.md) helps produce. Copy `fidelity-battery-template.json` from the skill folder, attach contact sheets/comparisons, and mark unsupported checks `not_testable` rather than guessing.

## Install

Each skill is a folder containing its activation metadata and complete procedure. Install the full family to use the router's cross-skill links; a focused leaf can also be installed alone when its input data is already prepared.

```sh
mkdir -p ~/.agents/skills
cp -R skills/source-artwork* ~/.agents/skills/
```

For project-only use, copy the skill folders into the project-level skills directory supported by your host. Discovery paths differ across agent products; the skills do not require a particular provider, renderer, editor, language, or runtime.

## License

MIT. See [LICENSE](LICENSE).
