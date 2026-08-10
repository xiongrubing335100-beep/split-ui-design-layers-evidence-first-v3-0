# Generation Target And Single Provenance

## Generation Target

Assign one target before generation:

- `complete-isolated-asset`
- `visible-fragment`
- `repaired-background-plate`

### complete-isolated-asset

Require a complete reusable subject or approved group. Include visually
necessary hidden continuation in the same first ImageGen result. Reject missing
owned parts, occluder-shaped cuts, or invented substitute parts.

### visible-fragment

Use only after the user explicitly accepts a visible fragment rather than a
complete reconstruction.

### repaired-background-plate

Preserve the source scene's meaning and style, remove foreground UI, and repair
only regions exposed by those removals.

## Single Provenance

Record:

- `source_reference`: original screenshot or original-context crop
- `output_provenance`: first ImageGen result
- `output_geometry`: generated-native
- `postprocessing`: none
- `original_pixel_overlay_allowed: false`
- `alpha_required: false`
- `delivery_background_mode: chroma-green | scene-background`
- `chroma_color: "#00FF00" | not-applicable`
- `chroma_generated_in_first_pass: true | not-applicable`
- `gray_intermediate_allowed: false`
- `background_conversion_allowed: false`

The original screenshot is a content and style reference. It is not an
authoritative pixel source for the delivered file.

## Approval Timing

Manifest confirmation authorizes low-risk completion only when the manifest
lists hidden parts, evidence, and risk. Request separate approval for ambiguous
anatomy, faces, branded detail, lettering, or complex identity structure.

## Geometry Boundary

Canvas size, aspect ratio, scale, position, and margins are advisory. Preserve
the first returned geometry and never align it back to the source screenshot.

## No Recursive Editing

Do not feed the first result back into ImageGen. If it fails a hard content
gate, mark it rejected or unresolved. Generate again only after a new explicit
user request names that asset.
