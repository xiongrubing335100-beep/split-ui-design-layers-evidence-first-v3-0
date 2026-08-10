# Blocking First-Result Acceptance Gates

Apply these gates to the first successfully returned ImageGen image. Technical
success never overrides an earlier content failure.

## 1. Ownership And Grouping

Verify that the output represents exactly one approved asset or approved group.
Reject unapproved controls, missing group members, added siblings, or internal
fragmentation of a compact identity asset.

## 2. Generation Target

Compare the output against `generation_target`.

- `complete-isolated-asset` requires a complete reusable subject or group.
- `visible-fragment` requires explicit user acceptance of incompleteness.
- `repaired-background-plate` requires the intended scene with foreground UI
  removed and exposed regions repaired.

## 3. Content And Identity

Compare with `expected_owned_parts`. Reject changes to:

- object category and meaningful sub-object count
- silhouette, openings, and negative spaces
- face, costume, product, prop, or container structure
- logo, emblem, packaging, embedded text, or identity mark
- distinctive colors, state, direction, and front-to-back order

## 4. Coverage

Verify that every expected owned part and every member of an approved group is
present. Reject flat occluder-shaped cuts, missing detached parts, duplicated
parts, or invented substitutes.

## 5. Foreign-Component Exclusion

Reject any surviving unapproved UI component, text fragment, icon, character,
badge, label, container, scene fragment, or unrelated decoration. Foreign
content is a hard failure regardless of canvas size, subject scale, or position.

## 6. Subject-Attached Texture Contamination

Reject gray, purple, patterned, shadow-like, or dirty generated texture when it
overwrites, touches, or appears fused into the intended subject. Reject duplicate
halos, doubled contours, floor shadows, or material smears.

## 7. First-Pass Green Screen

For foreground assets, require the first ImageGen result itself to use an
opaque full-frame solid `#00FF00` green screen. Reject:

- gray, white, checkerboard, transparent, or scene background regions
- gradient, texture, vignette, floor plane, cast shadow, or reflection
- green spill, reflection, or transmitted green inside the subject
- evidence that a gray or other background was converted to green afterward

For glass, bubbles, glow, or translucent subjects, preserve neutral material,
intrinsic transparency cues, iridescent rims, highlights, and attached glow
without showing the green plate inside the material.

For a background asset, evaluate the repaired scene instead of a gray plate.

## 8. Native File Preservation

Verify that the delivered file is the first returned PNG with its native width,
height, aspect ratio, subject position, scale, margins, and full canvas.

Reject any derived cutout, transparent PNG, Alpha file, source overlay,
registered composite, resized version, crop, or normalized export.

## Advisory Geometry

Never fail or retry because of:

- canvas width or height
- aspect ratio
- subject position
- subject scale
- margins

## One-Shot Status Calculation

Use:

- `accepted-first-result`
- `rejected-content-or-identity`
- `rejected-foreign-component`
- `rejected-incomplete-target`
- `rejected-background-intrusion`
- `rejected-green-screen-contract`
- `unresolved`

Never prefill acceptance. Allow zero automatic content-repair attempts. A new
generation requires a later explicit user request naming the failed asset.
