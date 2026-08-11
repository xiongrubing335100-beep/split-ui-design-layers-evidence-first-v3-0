# Candidate Execution

Read this reference only after the user authorizes all layers or named layer
IDs. Create one full-source-canvas candidate per explicitly authorized round,
then stop that layer for visual review.

## Reusable Full-Canvas Prompt

Fill every slot. Use the source screenshot as the only visual authority.

```text
Use the supplied UI screenshot as the only visual authority. Generate one full-source-canvas candidate in the original [SOURCE_WIDTH] × [SOURCE_HEIGHT] coordinate system and [SOURCE_ASPECT_RATIO] aspect ratio. Do not crop, trim, recenter, rescale, translate, rotate, redesign, beautify, simplify, sharpen, symmetrize, modernize, or restyle source-visible content.

Layer: [LAYER]
Position: [POSITION]
Included content: [INCLUDED_CONTENT]
Excluded neighbors: [EXCLUDED_NEIGHBORS]
Observed non-delivery text: [OBSERVED_NON_DELIVERY_TEXT]
Source-evidenced support effects: [SUPPORT_EFFECTS]
Locked visible anchors: [LOCKED_VISIBLE_ANCHORS]
Repair envelope: [REPAIR_ENVELOPE]
Fill colour: [FILL_COLOUR]
Compositing mode: [COMPOSITING_MODE]

Retain only the included layer and its evidenced support effects at the original coordinates. Remove the excluded neighbors and observed non-delivery text. The observed text is evidence and an occluder, not delivery content. Complete only the declared hidden segment or text-covered base area inside the repair envelope plus its stated minimal seam band. Preserve every locked visible anchor in identity, wording, glyph construction, geometry, colour, material, scale, orientation, and placement. Do not invent effects or merge neighboring UI. Outside the requested layer, use the uniform declared fill colour. This is the only requested layer.
```

Use `#00FF00` and `Normal` for opaque UI graphics. Use `#000000` for isolated
light effects whose declared compositing mode is `Screen` or `Add`.

## Authorization Rounds

Interpret `execution.max_candidates_per_layer: 1` as one returned candidate per
explicitly authorized round. Keep `execution.auto_retry: false` in every round.

For an initial selection, set `authorization_round: 1`,
`round_authorized: true`, and preserve the user's selection in
`authorization_request`. For an explicit retry request naming a `REJECTED`
layer, perform this transition in order:

1. Append the completed current round to `authorization_history`, including its
   round number, authorization request, candidate path, generation and transport
   counters, visual verdict, failure codes, terminal status, and review note.
2. Keep the archived candidate file at its recorded path. Never overwrite,
   delete, or silently discard prior candidate evidence.
3. Increment `authorization_round` by one and record the new explicit retry
   request in `authorization_request`.
4. Set `round_authorized: true`, `generation_count: 0`,
   `transport_retry_count: 0`, `candidate_path: null`, reset
   `visual_inspection` to its unperformed shape, clear `review_note` and
   `final_output`, and set `status: PENDING`.
5. Only after all four mutations succeed may the new round enter the
   one-candidate fuse.

Do not reset a rejected record without first appending its audit snapshot. Do
not treat an automatic process, vague retry request, or retry of an unnamed
layer as authorization for another round.

## One-Candidate Fuse

1. Verify that the selected manifest record is `PENDING`, has
   `round_authorized: true`, has `authorization_round` greater than `0`, and has
   `generation_count: 0` for that round.
2. Submit the completed prompt once with the source image.
3. Treat a returned image as the round's one candidate, even if it is poor.
4. Save it to a unique round-specific path such as
   `candidates/A07_round-02_candidate-01.png`; never overwrite an earlier round.
   Set `generation_count: 1`, record its path, and set
   `status: CANDIDATE_CREATED`.
5. Inspect the actual candidate against the source. Dimensions and prompt text
   do not count as visual inspection.
6. Never auto-retry, auto-repair, or generate alternatives. Require a later
   explicit user instruction naming the rejected layer to open another round.

A transport failure with no returned image leaves `generation_count: 0` and may
increment `transport_retry_count` once within the same authorization round. If
that transport retry returns an image, it is the round's one candidate. If it
still returns no image, set `BLOCKED`. After any image is returned,
`generation_count` is `1` and no transport or content retry is legal in that
round. Never call a second content generation because the first candidate failed
a release gate.

## Invalid Fallbacks

Do not build or invoke an automatic matting, segmentation, masking, or Alpha
estimation pipeline. The following are invalid fallbacks for both fully visible
and occluded UI layers:

- Rembg
- GrabCut
- colour threshold or colour-range separation
- manual polygon or hand-drawn ownership masks

Do not iterate local crops, thresholds, polygons, masks, edge cleanup, or Alpha
estimation. If the required image-generation route is unavailable, set the
selected layer to `BLOCKED`, report why, and stop it.
