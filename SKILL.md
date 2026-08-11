---
name: split-ui-design-layers-evidence-first-v3-0
description: Use when splitting UI-heavy campaign screens, game-event interfaces, H5 pages, or mini-app promotional designs into independently editable full-canvas UI layers while ordinary text remains recognized but excluded from delivery and candidates require evidence-based visual review.
---

# UI Evidence-First Layering V3.0

## Core Principle

Use UI semantics to decide what to split. Use evidence, full-canvas candidates,
and release gates to decide what can be delivered.

Preserve source-visible identity. Treat semantic similarity, beautification, and
generic UI conventions as insufficient evidence.

## Required References

Read the references at these gates:

- Before planning, read [UI component units](references/ui-component-units.md)
  and [text evidence and effects](references/text-evidence-and-effects.md).
- After authorization and before generating, read [candidate
  execution](references/candidate-execution.md).
- Before inspecting or delivering any candidate, read [release gates and
  delivery](references/release-gates-and-delivery.md).

Copy [the manifest template](assets/layer-manifest.json) into the output package
and update it as the single state record. Do not add pixel masks, Alpha
estimation, or exact-object bounding boxes to the planning contract.

## Inspect and Plan

1. Inspect the supplied screenshot at native resolution. Record its path,
   dimensions, aspect ratio, and visible UI regions.
2. Exclude device and operating-system chrome. Build a UI region/component tree
   before assigning layer IDs; do not substitute poster subject categories.
3. Record all visible text in evidence. Exclude ordinary UI text from default
   delivery, but retain its owner and occlusion relationships.
4. Create one manifest record for every eligible UI delivery layer. Describe
   position with source coordinates or an approximate source region; do not
   require a mask or pixel-tight bounding box.
5. Declare ownership, excluded neighbors, observed non-delivery text, supported
   effects, occlusions, completion regions, locked visible anchors, repair
   envelope, risk, fill colour, compositing mode, and front-to-back z-order.
6. Present an ID-annotated component tree before the proposed-record table.
   Prefix every eligible delivery node with the same manifest `Axx` ID used in
   the table. Show ordinary text inline as `[无 ID · excluded-default]`; do not
   assign IDs to text or region/category containers.

## Authorization Gate

Do not generate during inspection or planning. End the plan with exactly:

```text
请选择下一步：
1. 全部生成
2. 生成部分（请回复编号，例如：01、03、06）
3. 调整拆分清单（请说明要增删、合并或改名的项目）
```

Treat an unambiguous reply as authorization only for the selected IDs and their
declared routes. Open authorization round `1` for each initially selected ID.
A route change, text-layer addition, merge, or retry requires new explicit
authorization. A request naming a rejected layer opens its next authorization
round only after the prior candidate and verdict are archived in manifest
history.

## Execute One Candidate

For each `PENDING` layer in an active, explicitly authorized round:

1. Use the screenshot as the only visual authority and request one candidate on
   the full-source-canvas at original dimensions and coordinates.
2. Retain only the declared UI component and source-evidenced support effects.
   Remove its declared neighbors and observed non-delivery text.
3. Complete only the declared hidden segment or text-covered base area plus its
   minimal seam band. Preserve every locked visible anchor.
4. Use uniform `#00FF00` outside normal opaque UI layers and `#000000` outside
   Screen/Add light-effect layers.
5. Save the result under a round-specific path in `candidates/`, set the round's
   `generation_count` to `1`, and mark it `CANDIDATE_CREATED`. Inspect the actual
   image against the source.

Generate at most one candidate per explicitly authorized round and
never auto-retry. A transport failure that returns no image may be retried once as
transport only; it does not open another round or permit a second returned
candidate. If the required generation route is unavailable, mark the layer
`BLOCKED` and stop it. Never replace generation with local matting, masking,
thresholding, or polygon work.

## Review and Deliver

Apply every hard release gate to the candidate. Reject source-identity changes,
invented effects, ordinary-text residue, neighboring UI leakage, component
bundling, canvas or key-field defects, layout drift, or repairs outside the
declared envelope. Record failure codes and mark the layer `REJECTED`; continue
only with other already-authorized IDs.

Use `NEEDS_USER_REVIEW` only when no hard failure exists and a subjective art
direction choice remains. Require explicit acceptance before setting `ACCEPTED`
and moving a candidate into `layers/`. Treat an explicit request naming a
rejected layer as authorization for a new round, not as a direct generation
command: archive the rejected round, increment the round, clear current
candidate/review fields, reset round counters, return to `PENDING`, and only then
execute one new candidate.

Keep candidates and accepted layers at full source dimensions. Composite from
the largest z-order index down to the smallest and audit the stack for duplicate
content, missing components, leaked UI, invented effects, and source-layout
drift before delivery.
