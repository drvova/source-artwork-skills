# Source Artwork Skills

Portable, provider-agnostic agent skills for inspecting real artwork and recreating its 2D animation and 3D form faithfully.

## Skills

- **[Source Artwork 2D/3D Reconstruction](skills/source-artwork-2d-3d-reconstruction/SKILL.md)** — unified workflow; use this for end-to-end, source-faithful 2D and 3D work.
- [Source Artwork 2D Animation](skills/source-artwork-2d-animation/SKILL.md) — focused pixel-art, SVG, sprite-frame, and playback workflow.
- [Source Artwork 3D Animation](skills/source-artwork-3d-animation/SKILL.md) — focused source-derived 3D geometry, rigging, and looping animation workflow.

## Website examples

These live examples show different parts of the reconstruction workflow:

- **[Foresight Labs](https://foresight-labs.eu/)** — the earlier site build researched for this project used a frame-by-frame axolotl animation: 24 frames on a 39 × 28 pixel-cell grid, decoded and rendered as SVG. The live homepage has since changed, so this is a documented reverse-engineering case study, not a promise that the same animation is still displayed.
- **[Rive: Old Man Character Animation](https://rive.app/marketplace/28375-53649-old-man-character-animation-or-rive-interactive-v02/)** — a rigged 2D character example for studying expressive features, blinking, facial changes, and interactive state-machine animation.
- **[Three.js: Skinning and Morphing](https://threejs.org/examples/webgl_animation_skinning_morph.html)** — a 3D character example for studying animation clips, looping, crossfades, skeletal deformation, and facial morph targets.

Use the examples to study technique and validation targets; the skills still require inspecting each page's current assets and behavior instead of assuming its implementation.

## What “1:1” means here

When the original 2D paths, frames, or model are available, the skills direct the agent to preserve them and verify against matched captures. If only a flat 2D reference exists, hidden 3D depth and surfaces cannot be recovered exactly; the unified skill requires those parts to be identified as inferred rather than presented as the original model.

## Install

Each skill is a self-contained folder containing a standard `SKILL.md` with a name and activation description. Install the folder—not just the Markdown file—into a skills directory supported by your agent. For example, in an environment that loads user skills from `~/.agents/skills/`:

```sh
mkdir -p ~/.agents/skills
cp -R skills/source-artwork-2d-3d-reconstruction ~/.agents/skills/
```

For project-only use, copy it into the project-level skills directory supported by your host. Skill-discovery paths differ between agent products; this repository does not depend on any particular model provider, renderer, editor, or agent runtime.

## License

MIT. See [LICENSE](LICENSE).
