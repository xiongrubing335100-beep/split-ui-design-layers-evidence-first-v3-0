# Release Gates and Delivery

Read this reference before evaluating a candidate. Compare the actual candidate
against the source at native dimensions. A visually attractive or semantically
similar result does not pass a source-evidence gate.

## Hard Failure Codes

Reject a candidate and record every applicable code:

| Code | Hard failure |
| --- | --- |
| `ICON_IDENTITY_MUTATION` | An icon's silhouette, identity, internal construction, material, colour, sharpness, symmetry, or source-visible details changed. |
| `TEXT_OR_TITLE_MUTATION` | Delivered title/text wording, glyph construction, spacing, baseline, hierarchy, source-evidenced effects, or placement changed. |
| `EFFECT_INVENTION` | An unsupported glow, shadow, bevel, extrusion, thickness, backing, brush, border, or decoration appeared, or an outer glow became a dimensional effect. |
| `ORDINARY_TEXT_RESIDUE` | Text declared `excluded-default` survives in the candidate. |
| `UI_BUNDLING_ERROR` | Independently movable UI components were merged, or an icon/base/title absorbed an unapproved sibling. |
| `GEOMETRY_DRIFT` | Source-visible UI geometry, scale, orientation, registration, placement, crop, or relative layout drifted. |
| `REPAIR_ENVELOPE_EXCEEDED` | Generated completion extends beyond the declared hidden segment and minimal seam band, or changes a forbidden region. |
| `FOREIGN_UI_LEAKAGE` | An excluded button, plaque, icon, label, strip, progress control, motif, or other neighboring UI remains or is invented. |
| `BACKGROUND_DRIFT` | Visible background perspective, panel line, horizon, motif, structure, grade, or colour relationship changed. |
| `VISIBLE_ANCHOR_MUTATION` | Any declared locked visible anchor changed in identity, wording, geometry, colour, material, scale, orientation, or placement. |
| `COMPLETION_MISSING` | A declared hidden segment or text-covered base remains blank, keyed out, discontinuous, or still contains the removed occluder. |
| `CANVAS_OR_KEY_FIELD_INVALID` | Dimensions/aspect ratio differ from the source, the layer is cropped/recentered, or the green/black key field is non-uniform or contaminated. |

Treat any listed code as terminal for that candidate and authorization round.
Set `REJECTED`, preserve the candidate and review note, and do not retry
automatically.

For a review-only decision, report the terminal fields explicitly before the
explanation:

```text
status: REJECTED
failure_codes: [APPLICABLE_HARD_FAILURE_CODES]
auto_retry: false
```

Do not replace the `REJECTED` status token with only a prose synonym.

## Status Model

- `PENDING`: planned but not yet generated. Execute only when
  `round_authorized` is true, `authorization_round` is greater than `0`, and the
  current round's `generation_count` is `0`.
- `CANDIDATE_CREATED`: the single candidate exists and awaits or is undergoing
  visual inspection; the current round's `generation_count` is `1`.
- `REJECTED`: at least one hard failure code applies. Require an explicit retry
  instruction naming this layer to open a new authorization round before any new
  attempt.
- `NEEDS_USER_REVIEW`: no hard failure applies, but a subjective art-direction
  decision remains. Do not use this status to bypass uncertainty about source
  fidelity.
- `ACCEPTED`: all hard gates pass and the user explicitly accepts this candidate.
- `BLOCKED`: an authorized route or required tool is unavailable, authorization
  is insufficient, or a transport-only retry still returns no image.

Do not move directly from `CANDIDATE_CREATED` to `ACCEPTED` on automated review
alone. User acceptance is a separate gate.

## Explicit Retry Transition

Keep `auto_retry: false`. Define `max_candidates_per_layer: 1` as one candidate
per explicitly authorized round, never as permission to regenerate inside a
round.

When the user explicitly names a `REJECTED` layer for retry:

1. Append an immutable snapshot of the current round to
   `authorization_history`. Include `authorization_round`,
   `authorization_request`, `generation_count`, `transport_retry_count`,
   `candidate_path`, `visual_inspection` with failure codes, `status`, and
   `review_note`.
2. Preserve the candidate at the archived path.
3. Increment `authorization_round`; set `round_authorized: true`; store the new
   user request in `authorization_request`.
4. Clear current candidate state: set `generation_count: 0`,
   `transport_retry_count: 0`, `candidate_path: null`, unperformed
   `visual_inspection`, `review_note: null`, `final_output: null`, and
   `status: PENDING`.
5. Create at most one new candidate only after this transition completes.

Never overwrite an archived snapshot or reuse its candidate path. A transport
retry with no returned image stays inside the current authorization round and
does not append history, increment `authorization_round`, or permit two returned
candidates.

## Delivery Package

Create this package unless the user supplies another destination:

```text
outputs/ui_layers/<source-stem>/
├── source.png
├── layer_manifest.json
├── candidates/
└── layers/
```

- Keep every candidate and accepted layer at the full source dimensions and at
  the original coordinates.
- Keep raw candidates in `candidates/`. Copy or move only explicitly accepted
  candidates into `layers/`.
- Normalize accepted files only with non-generative fixed-canvas operations.
  Do not crop, stretch, rotate, content-fill, redraw, or repair during delivery.
- Record front-to-back z-order and composite from the largest index down to the
  smallest.
- Audit recomposition for independently movable UI, evidenced support effects,
  continuous declared completion, locked-anchor fidelity, duplicate content,
  leaked UI, invented effects, and source-layout drift.
- Report every authorized ID with its status, candidate or final path, failure
  codes, and concise review note. Do not silently omit rejected or blocked IDs.
