# First-Generation Asset Method Matrix

Apply `manifest-eligibility-filter.md` and confirm the manifest before choosing
any method.

## Single Generation Method

Use one ImageGen edit for every selected asset. Provide the original screenshot
or an original-context crop as the visual reference. Request the complete
approved asset or group in one image.

The first successfully returned image is the final candidate. Do not create an
underlay, source overlay, matte, cutout, Alpha channel, registered composite, or
normalized export.

## Prompt Focus By Asset Kind

| Asset kind | Preserve | Exclude |
|---|---|---|
| character or mascot | identity, face, costume, pose, props, direction, attached effects | all other UI, text, characters, badges, and scenery |
| illustrated icon or reward | emblem, internal object count, openings, glow, material, identity marks | labels, neighboring icons, containers, unrelated particles |
| blank UI base | complete silhouette, gradient, rim, bevel, backplate, attached ornament | ordinary label text, values, other controls |
| grouped set | exact approved membership, count, order relationships, shared material | every non-member component |
| background plate | original scene style and repaired exposed regions | foreground UI, text, characters, and controls |

## First-Pass Green Screen

Request an opaque full-frame solid `#00FF00` green screen in the first and only
ImageGen result for every foreground asset. Do not generate gray, white,
checkerboard, transparent, or scene backgrounds first, and do not convert a
later background to green.

Require no gradient, texture, vignette, floor plane, cast shadow, reflection,
or green spill. For glass, bubbles, glow, or translucent assets, keep the
material neutral and preserve intrinsic highlights and iridescent rims without
showing the green plate inside the subject.

Use the repaired scene itself for a background-plate asset.

## Fidelity

Use:

- `identity-faithful` for characters, mascots, products, props, logos, and
  illustrated rewards
- `material-faithful` for UI bases and containers
- `semantic-faithful` for backgrounds and broad ambience

Never describe the generated output as exact source-pixel fidelity.

## Native Output Preservation

Keep the returned width, height, aspect ratio, margins, subject scale, and
subject position. Copy the returned PNG unchanged whenever possible.

Do not retry or post-process for geometry differences. Apply the content gates
and one-shot fuse instead.
