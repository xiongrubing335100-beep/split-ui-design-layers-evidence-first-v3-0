# Text Evidence and Effects

Read this reference before planning. Separate recognition from delivery: visible
text always participates in ownership and occlusion reasoning, even when it does
not become an asset.

## Text Evidence Record

Record every visible text item with these fields:

```yaml
observed_text: "verbatim text, or a faithful unreadable/partial notation"
text_role: "navigation|explanation|record|date|time|value|counter|price|cost|notice|task|shortcut-label|button-copy|title|identity|other"
owner_component: "region/component/path"
occludes_component: ["component paths whose useful pixels it covers"]
delivery_policy: excluded-default
```

- `observed_text` records what is visibly present; never pretend excluded text
  is absent.
- `text_role` describes the UI function, not a delivery decision.
- `owner_component` names the component that displays or contains the text.
- `occludes_component` names every useful base or graphic covered by the text.
- `delivery_policy` controls delivery without deleting evidence.

## Delivery Policies

Use `delivery_policy: excluded-default` for ordinary UI text, including:

- navigation, menu, explanation, record, rule, task, shortcut, and button copy
- dates, times, values, counters, percentages, prices, and costs
- notices, captions, labels, and text badges

Do not assign these items layer IDs, count them as delivery assets, or include
them with adjacent icons. If ordinary text covers a useful plaque, button, or
other blank base, record it as an occluder and propose the blank base as the
delivery layer.

Use `delivery_policy: top-kv-title` for at most one dominant campaign title. Keep
the title as one visual group with only its source-evidenced stroke, glow,
shadow, bevel, or extrusion.

Use `delivery_policy: explicit-text` only after the user explicitly opts in to a
specific text or identity layer. Approval for an icon, base, logo mark, or
region does not authorize its adjacent text. Record the exact requested text;
do not broaden opt-in to other copy.

## Effect Evidence

Describe only effects that native-resolution source evidence supports:

| Term | Required visible evidence |
| --- | --- |
| `stroke` | A crisp boundary that expands around the silhouette. |
| `outer-glow` | A diffuse, non-directional exterior halo with soft falloff and no solid side face. |
| `drop-shadow` | Directionally displaced darkness without a continuous side wall. |
| `bevel` | Highlight and shading structure inside the object boundary. |
| `extrusion` | Continuous directional side faces with hard edges and consistent depth. |

A bright outline alone is not extrusion. A diffuse halo is `outer-glow`, not
thickness. A shadow, glow, antialiasing, or isolated offset pixel is not evidence
of extrusion. Use `unknown` when the evidence is ambiguous.

Attach an effect to its owner when it serves only that owner. Separate an effect
only when it has an independent z-order, spans multiple components, or can move
or animate independently. Never add bevel, glow, shadow, thickness, backing
plate, brush, or extrusion merely because it is common in polished UI.
