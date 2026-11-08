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
        <img src="examples/axolotl-2d-animation.svg" alt="A warm-toned pixel-cell axolotl with upright orange gills, dark eyes, a pale face, and splayed limbs.">
      </picture>
    </td>
    <td>
      <picture>
        <source srcset="examples/axolotl-3d-demo-still.png" media="(prefers-reduced-motion: reduce)">
        <img src="examples/axolotl-3d-demo.gif" alt="A low-resolution 3D axolotl with a cream body, orange gills, dark eyes, and splayed limbs against a black background.">
      </picture>
    </td>
  </tr>
</table>

The left SVG is an authored 2D rig; the right GIF is a 3D reconstruction from the same visible source cells. The static `<source>` branches supply stills when reduced motion is preferred.

1. **Trace the source page.** The Foresight Labs page build used for this case study exposed a separate 2D animation. The source author and upstream license were not recorded.
2. **Inspect the static mark.** The input was a 312 × 266 SVG with seven colored paths containing 253 rectangular pixel-art cells. Preserve the original path geometry, palette, and cell registration; distinguish this static mark from the site's animation player.
3. **Recover the motion data.** The inspected site bundle contained 24 base64-packed frames on a 39 × 28 cell grid. Its runtime decoded the frames to SVG and played them at 12 fps; hover and keyboard focus drove playback. Trace the actual data consumer, decode the frames, and validate order, registration, and timing rather than guessing the format.
4. **Separate recovered motion from adaptation.** The original frame payload is not included here. The 2D example is a new source-cell rig, not a frame-for-frame export of the site's 24 frames.
5. **Author the 2D cycle.** Keep the cells and colors intact; group the gills and limbs around attachment pivots, then add a restrained body sway and blink. The example intentionally gives the left and right gills different motion ranges; that asymmetry is authored, not recovered from the site's frame data. Give the animated SVG and still fallback one shared padded viewBox so the motion stays in frame and reduced-motion selection does not resize the art; inspect extreme poses for silhouette drift.
6. **Derive the 3D form.** Extrude the visible cells into shallow layers and group the body, gills, eyes, and limbs around observed attachment points. The source is flat, so thickness, back surfaces, and hidden anatomy are inferences; any lighting-derived color changes are authored, not recovered source data.
7. **Render and verify.** Capture matched source/output views, inspect registration, color boundaries, joints, loop seams, and motion preferences, then record the claim with the [Capture Harness](skills/source-artwork-capture-harness/SKILL.md) and [Fidelity Battery](skills/source-artwork-fidelity-battery/SKILL.md). The editable 2D SVGs are included; the 3D model source and GIF/PNG generation command are not, so the 3D media are explicitly incomplete, non-reproducible presentation fixtures, not clean-regeneration targets.

**Fidelity and rights boundary:** the historical website animation was 2D, and its frame format and playback behavior were inspected. The 2D animation here is a new source-cell rig; the 3D GIF is inferred from visible 2D artwork, not an original 3D model recovered from the site. The axolotl source and its derivatives are excluded from this repository's MIT grant. Their original author and upstream license were not recorded.


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
        <img src="examples/butterfly-animation.gif" width="320" height="256" alt="An orange pixel-cell butterfly above a green meadow, beneath pale clouds and a yellow sun.">
      </picture>
    </td>
    <td>
      <picture>
        <source srcset="examples/jellyfish-animation-still.png" media="(prefers-reduced-motion: reduce)">
        <img src="examples/jellyfish-animation.gif" width="320" height="256" alt="A lavender pixel-cell jellyfish with a rounded bell and four pale tentacles against a dark blue underwater scene.">
      </picture>
    </td>
  </tr>
</table>

Both are original, MIT-licensed demonstrations built from explicit 8 × 8 SVG cells—not animations recovered from a website. The butterfly rigs its two wings around their attachment pivots; the jellyfish coordinates a bell pulse with four separate tentacle sweeps. The editable [butterfly SVG](examples/butterfly-animation.svg) and [jellyfish SVG](examples/jellyfish-animation.svg) are their animation sources; the PNGs are still exports, and the GIFs are palette-quantized motion previews rather than pixel-identical SVG captures.

## What “1:1” means here

Fidelity verdicts apply only to the tested view and claims. When a native 3D source model exists, a whole-object 3D equivalence claim also requires source-value geometry checks; matching one rendered view is not enough. If only a flat 2D reference exists, hidden 3D depth and surfaces cannot be recovered exactly; the unified skill requires those parts to be identified as inferred rather than presented as the original model.

## Quantified Fidelity Check

Use the [Fidelity Battery](skills/source-artwork-fidelity-battery/SKILL.md) after reconstruction when you need a reviewable pass/fail result. It defines comparison modes, check scope, measurement formulas, and the only numeric tolerance defaults; the [JSON template](skills/source-artwork-fidelity-battery/fidelity-battery-template.json) is the proof packet. Mark unsupported checks `not_testable` with the evidence gap; a required untestable check cannot support an unqualified 1:1 verdict.

## Install

Each skill is a folder containing its activation metadata and complete procedure. Install the full family to use the router's cross-skill links; a focused leaf can also be installed alone when its input data is already prepared.

```sh
mkdir -p ~/.agents/skills
cp -R skills/source-artwork* ~/.agents/skills/
```

For project-only use, copy the skill folders into the project-level skills directory supported by your host. Discovery paths differ across agent products; the skills do not require a particular provider, renderer, editor, language, or runtime.

## License

MIT applies to the skills, documentation, and original butterfly/jellyfish examples. It does not apply to the `examples/axolotl-*` source artwork or derivatives. The historical source reference is [Foresight Labs](https://foresight-labs.eu/); the upstream author and license were not recorded. See [LICENSE](LICENSE) for the scope note and terms.
