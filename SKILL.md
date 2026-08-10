---
name: split-ui-design-layers-semantic-first-v1-8
description: "Split UI-heavy screenshots, H5 activity pages, mini-app pages, illustrated campaigns, and game interfaces into semantically isolated asset images. Use when Codex must analyze and confirm a filtered asset manifest, generate all or selected asset IDs once with ImageGen directly on an exact solid #00FF00 green-screen background, preserve the first generated image at its native returned canvas and geometry, deliver that opaque PNG unchanged, exclude foreign UI components, and avoid gray intermediates, background conversion, cutout, Alpha extraction, original-pixel overlay, registration, and transparent-PNG production."
---

# Semantic-First UI Asset Split V1.8

## Goal

Turn one UI screenshot into a confirmed set of semantically isolated asset
images. Use the original screenshot as the content and style reference. For
each selected asset, make exactly one ImageGen edit and treat the first returned
image as the final delivery candidate.

For every foreground asset, require the first ImageGen result itself to use an
opaque solid `#00FF00` green-screen background. Deliver that first generated PNG
on its native returned canvas. Do not create a gray intermediate, convert any
background to green, cut it out, derive Alpha, overlay original pixels, align it
back to the source, resize it, crop it, or normalize it to the source canvas.

Prioritize:

1. correct asset identity and owned-part count
2. user-approved grouping
3. absence of foreign UI components
4. complete and coherent reconstructed content
5. faithful style, state, direction, and material
6. compliant first-pass green-screen background

Canvas size, aspect ratio, subject scale, and subject position are advisory.
They are never failure evidence.

## Required References

Read all references before planning or execution:

- `references/manifest-eligibility-filter.md`
- `references/atomic-editability-contract.md`
- `references/asset-method-matrix.md`
- `references/completion-target-and-provenance.md`
- `references/icon-and-blank-base-contract.md`
- `references/content-acceptance-gates.md`
- `references/first-generation-delivery-contract.md`

Load `$imagegen` only after the user authorizes all assets or selected IDs.

## Manifest Eligibility Filter

Apply the eligibility filter before building the component tree, assigning IDs,
counting assets, or presenting the manifest.

- Exclude system chrome, including time, signal, Wi-Fi, battery, privacy or
  recording indicators, gesture bars, and Home Indicators.
- Exclude ordinary default text, including subtitles, descriptions, dates,
  values, notices, labels, button copy, prices, percentages, and task copy.
- Allow at most one text exception: the dominant top-KV main-title visual group.
- Keep embedded identity text inside logos, products, packaging, badges, or
  illustrations with its owning graphic.
- Keep independently useful non-text containers as blank assets, without their
  removed label text.

Excluded elements must not appear in the user-visible component tree, asset
count, IDs, manifest, unresolved list, or delivery list.

Follow `references/manifest-eligibility-filter.md`.

## Non-Negotiable Contract

- Use the original screenshot as a visual reference, not as final output pixels.
- Generate one isolated image per approved delivery asset or approved group.
- Treat the first successfully returned ImageGen image as the final candidate.
- Preserve the returned file's native width, height, aspect ratio, subject
  scale, subject position, margins, and complete canvas.
- Copy the returned PNG unchanged whenever the tool provides a local file.
- Do not resize, crop, reposition, sharpen, denoise, repaint, or recompress the
  accepted first result.
- Do not overlay, paste, lock, or restore pixels from the original screenshot.
- Do not align or register the generated result to the original screenshot.
- Do not create a source mask, completion mask, acceptance mask, matte, or Alpha
  channel.
- Do not remove the generated background.
- Do not create a transparent PNG.
- Require the first ImageGen call for every foreground asset to generate the
  subject directly on a full-frame opaque solid `#00FF00` green screen.
- Do not generate a gray, white, checkerboard, transparent, or scene background
  before the green-screen result.
- Do not convert, recolor, replace, or repaint any background after generation.
- Do not feed the generated result into ImageGen as a new reference.
- Do not automatically repair or regenerate a content failure.
- Do not run an optional HD or enhancement pass without a new explicit request.

## First-Pass Green-Screen Contract

For every foreground asset, request the green screen in the first and only
ImageGen result:

- fill the complete background with opaque solid `#00FF00`
- use no gradient, texture, vignette, scenery, UI, text, checkerboard, floor
  plane, cast shadow, reflection, or unrelated decoration
- do not let green reflect, spill, or transmit into the subject
- for glass, bubbles, glow, or translucent subjects, keep the material neutral
  and preserve intrinsic transparency cues, iridescent rims, highlights, and
  attached glow without showing the green plate inside the material
- preserve shadow only when it intrinsically belongs to the asset; exclude
  background or contact shadows

Green-screen nonuniformity, gray or white background regions, checkerboard
pixels, and green contamination inside the subject are hard failures. Do not
repair them automatically or convert the background after generation.

For a background asset, generate the repaired scene background itself instead
of using green. Remove foreground UI and repair only the newly exposed regions.

## Atomic Asset Model

Build:

1. page region
2. component
3. component role
4. atomic asset
5. delivery asset or user-approved delivery group

Separate an asset when it can reasonably move, hide, change state, receive an
interaction, occupy a distinct z-order, or repeat as a sibling. Keep intrinsic
parts of a mascot, product, logo, reward, package, or illustrated identity
together.

Treat an explicit user grouping, exclusion, merge, or renaming instruction as
authoritative for the current task. When the user combines several visible
parts into one numbered asset, generate them together in one first result.

Follow `references/atomic-editability-contract.md`.

## Repeated Instances

Give meaningful repeated instances unique IDs only when the user wants them
independently editable. Group repeated items into one asset when the manifest or
the user's revision explicitly requests a combined set.

Never silently expand one selected ID into unselected sibling assets.

## Generation Target

Assign exactly one target to every manifest asset:

- `complete-isolated-asset`: generate the complete reusable subject or group,
  including visually necessary hidden continuation
- `visible-fragment`: reproduce only the visible fragment when the user
  explicitly accepts incompleteness
- `repaired-background-plate`: remove foreground UI and complete exposed scene
  regions

Use a single provenance record:

- `source_reference`: original screenshot or original-context crop
- `output_provenance`: first ImageGen result
- `output_geometry`: generated-native
- `postprocessing`: none

Never claim exact source-pixel fidelity for the generated result.

Follow `references/completion-target-and-provenance.md`.

## Manifest

Create one record per eligible delivery asset before generation. Include:

- `asset_id`, `asset_name`, `component_path`, `role`
- `instance_id`, `repeat_group_id`, and `delivery_group_id` when applicable
- `owns`, `must_exclude`, `expected_owned_parts`
- `identity_features`, `state_features`, `direction_features`
- `generation_target`, `hidden_parts`, `completion_risk`
- `source_reference`
- `delivery_background_mode: chroma-green | scene-background`
- `chroma_color: "#00FF00" | not-applicable`
- `chroma_generated_in_first_pass: true | not-applicable`
- `gray_intermediate_allowed: false`
- `background_conversion_allowed: false`
- `output_geometry: generated-native`
- `postprocessing: none`
- `alpha_required: false`
- `original_pixel_overlay_allowed: false`
- `generation_attempt_limit: 1`
- calculated gate results, output path, failure reason, and terminal status

Present before execution:

- filtered component tree
- atomic asset count
- proposed delivery asset count
- every grouped asset and its owned parts
- generation target and hidden parts
- the three execution choices below

Do not include an output-directory plan as a substitute for the execution
choices.

## Manifest Execution Choice

End every pre-execution manifest response with exactly these three choices:

```text
请选择下一步：
1. 全部生成
2. 生成部分（请回复编号，例如：01、03、06）
3. 调整拆分清单（请说明要增删、合并或改名的项目）
```

Do not generate until the user selects one. If the user already makes an
unambiguous selection, act on it without requesting a second confirmation.

For partial generation, accept commas, spaces, Chinese enumeration commas, and
ID prefixes such as `A06`. Validate IDs against the latest manifest and process
only the selected IDs.

For manifest revision, update the complete manifest and show the same three
choices again. Do not generate during a revision-only turn.

## ImageGen Method

For each selected asset:

1. provide the original screenshot or an original-context crop
2. name exactly one approved asset or approved group
3. list every expected owned part
4. list every component, text fragment, and decoration that must be absent
5. request any necessary hidden continuation as part of the complete subject
6. request a direct first-pass solid `#00FF00` green screen for every foreground
   asset, or the repaired scene for a background asset
7. request one image only

Use the first successfully returned image as the only candidate. Do not perform
a second ImageGen edit to improve it automatically.

Follow `references/asset-method-matrix.md` and
`references/first-generation-delivery-contract.md`.

## Asset-Specific Guidance

- `character or mascot`: preserve face, costume, pose, held props, direction,
  meaningful anatomy, and attached effects
- `illustrated icon or reward`: preserve emblem, internal object count,
  openings, glow, highlights, and identity marks
- `blank UI base`: remove ordinary label text and reconstruct the entire owned
  material, including gradient, rim, bevel, backplate, and attached decoration
- `grouped set`: preserve the approved member count and membership; do not add
  or omit members
- `background plate`: preserve the scene style and repair only regions exposed
  by removed foreground assets

Follow `references/icon-and-blank-base-contract.md`.

## Acceptance Contract

Run gates in this order:

1. asset ownership and approved grouping
2. generation-target satisfaction
3. content and identity
4. expected owned-part and group-member coverage
5. foreign-component exclusion
6. subject-attached generated-texture contamination
7. first-pass green-screen suitability
8. file validity and native-output preservation

Hard failures include:

- wrong asset identity, state, direction, or meaningful part count
- missing or invented owned parts
- foreign UI components, text, icons, characters, badges, or decoration
- duplicated contours or unrelated material attached to the subject
- a background or floor shadow fused into the intended asset

Advisory differences include:

- returned canvas width or height
- aspect ratio
- subject scale
- subject position
- margins

Use only:

- `accepted-first-result`
- `rejected-content-or-identity`
- `rejected-foreign-component`
- `rejected-incomplete-target`
- `rejected-background-intrusion`
- `rejected-green-screen-contract`
- `unresolved`

Never prefill an accepted status. Follow
`references/content-acceptance-gates.md`.

## One-Shot Fuse

- Allow exactly one successful ImageGen result per selected asset.
- Treat transport or tool failure with no returned image as no result; retrying
  the tool call does not count as a content regeneration.
- Allow zero automatic repair attempts.
- Allow zero masking, cutout, Alpha, compositing, registration, or normalization
  attempts.
- Allow zero gray-to-green conversion or other background-replacement attempts.
- Never retry because of canvas size, scale, position, aspect ratio, or margins.
- On a hard content failure, mark the asset rejected or unresolved and continue
  with the remaining selected IDs.
- Regenerate a failed asset only after a new explicit user request naming that
  asset.

## Workflow

1. Inspect the original screenshot at native resolution.
2. Apply the manifest eligibility filter.
3. Build the component tree and proposed delivery assets.
4. Apply explicit user groupings and exclusions before numbering.
5. Record owned parts, foreign exclusions, identity, state, and direction.
6. Assign a generation target and direct first-pass background mode.
7. Present the complete filtered manifest and the three execution choices.
8. Wait for all-generation, selected-ID generation, or manifest revision.
9. Load `$imagegen` after generation authorization.
10. Generate each selected foreground asset exactly once from original context
    directly on opaque solid `#00FF00`; never generate gray first.
11. Preserve each first returned PNG unchanged at its native generated size.
12. Run the content-focused acceptance gates.
13. Accept, reject, or mark unresolved without automatic repair.
14. Continue until every selected ID has one terminal status.
15. Deliver accepted first-result PNGs and a concise manifest or failure list.

## Delivery

Deliver:

- accepted first ImageGen PNGs with their original `#00FF00` green-screen
  backgrounds and native returned dimensions
- the filtered manifest with selected IDs and calculated statuses
- concise rejected or unresolved reasons

Do not deliver:

- transparent PNGs or derived Alpha files
- cutouts, mattes, masks, checkerboards, or derived chroma-key results
- original-pixel overlays or source/generated composites
- original-canvas-normalized versions
- resized, cropped, sharpened, denoised, or re-encoded variants
- restacked UI previews unless the user explicitly asks for one

Do not silently omit a selected asset. Keep failed raw candidates internal
unless the user asks to inspect them.
