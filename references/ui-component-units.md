# UI Component Units

Read this reference before creating the component tree or assigning delivery
IDs. Use it to decide what the UI layers are; use the execution reference later
to decide how authorized layers become candidates.

## Decision Order

1. Exclude device and operating-system chrome.
2. Identify the UI region: `background`, `top-kv`, `navigation`,
   `shortcut-entry`, `information-strip`, `primary-scene`, or
   `bottom-action-area`.
3. Identify independently editable or stateful components inside each region.
4. Separate a component when it has independent interaction, state, movement,
   repetition, z-order, visibility, recolouring, or reuse.
5. Split icon art from its blank base when either can move, change, hide,
   recolour, or be reused independently. Keep badges, decorative strips,
   progress controls, button bases, and separators as UI-domain units under the
   same test.
6. Split repeated sibling buttons, shortcut entries, tabs, and controls into
   distinct instances. Give each sibling a stable `instance_id`; repetition
   never justifies one merged delivery layer.
7. Keep a central illustration, apparatus, character, or identity graphic
   grouped when its internal parts form one inseparable visual identity.
   Separate attached controls or overlays that can move or change independently.
8. Attach an evidenced decoration to its owner when it serves only that owner.
   Separate it when it has an independent z-order, spans multiple controls, or
   can animate independently.

## Component Record

For every eligible unit, record:

- `ui_region`, `component_path`, `instance_id`, and front-to-back `z_order`
- `owns`, `excludes`, `occludes`, and `occluded_by`
- the source evidence that establishes its visible form and position
- `ui_independence: independent | attached` and a concrete separation reason
- source-evidenced support components and their owner
- locked visible anchors, completion regions, repair envelope, and risk

Treat a visible pixel as owned by one component. Assign generated completion
pixels to the component whose hidden structure they restore.

## ID-Annotated Planning Output

After eligibility filtering and manifest ID assignment, render the component
tree before the layer-record table. Prefix every eligible delivery node with
the same `Axx` ID used by its manifest record and table row. Put the ID at the
front of the node label so the tree can be scanned directly. Never renumber the
table independently from the tree.

Region/category containers receive no ID. Show ordinary text at its owner as
`[无 ID · excluded-default]`; it remains evidence and an occluder, not a
delivery asset. Do not leave an eligible delivery node unnumbered and do not
reuse one ID for sibling instances.

Use this output shape:

```text
应用内容
├─ top-kv
│  ├─ [A03] 主标题视觉组
│  └─ [无 ID · excluded-default] 活动日期文字
└─ navigation
   └─ 返回按钮
      ├─ [A01] 返回箭头
      └─ [A02] 返回按钮圆底
```

## Grouping Boundaries

- Keep the single dominant top-KV title as one title visual group with only its
  evidenced support effects.
- Keep identity-intrinsic illustration parts together only when separating them
  would destroy the central illustration's identity.
- Keep an icon separate from its adjacent ordinary label. The label remains
  text evidence unless explicitly requested for delivery.
- Keep independently movable navigation, CTA, shortcut, price, progress, badge,
  logo, and icon components separate even when they are visually aligned.

Do not reduce the screen to poster categories such as character, product,
headline, effects, and background. Region grouping is navigation for the tree,
not permission to bundle its independently editable children.
