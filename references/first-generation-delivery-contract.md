# First Generation Delivery Contract

Use this contract for every selected asset after manifest confirmation.

## Core Rule

The first successfully returned ImageGen PNG is the final delivery candidate.
Preserve its complete native canvas and pixels. The generated file is not an
underlay, intermediate repair source, mask source, or registration target.

## Allowed Operations

Allow only:

1. send the original screenshot or original-context crop to ImageGen
2. request one approved asset or approved group
3. receive one generated image
4. visually inspect it against the content gates
5. copy the returned file unchanged to the delivery directory
6. record its path, dimensions, attempt count, and status

## Forbidden Post-Processing

Do not:

- overlay or restore original screenshot pixels
- align, register, translate, or scale the generated subject
- crop or resize the generated canvas
- remove the background
- compute a mask, matte, segmentation, or Alpha channel
- use GrabCut, thresholding, or chroma key to derive a new image
- create a transparent PNG
- sharpen, denoise, recolor, repaint, or recompress the accepted file
- feed the result back into ImageGen
- create a restacked source composite as an acceptance requirement

## First-Pass Green Screen

For every foreground asset, request an opaque full-frame solid `#00FF00` green
screen in the first and only ImageGen result. Do not generate gray, white,
checkerboard, transparent, or scene backgrounds first. Do not replace, recolor,
or repaint the returned background afterward.

Keep the green plate free of gradient, texture, vignette, UI, text, scenery,
floor planes, cast shadows, and reflections. Prevent green spill, reflection,
or transmission inside the subject.

For glass, glow, bubbles, or translucent subjects, preserve neutral material,
intrinsic transparency cues, iridescent rims, highlights, and attached glow
without showing the green plate inside the material.

For background assets, deliver the first repaired scene background instead of a
green screen.

## Geometry

Treat returned canvas size, aspect ratio, subject scale, position, and margins
as advisory. Keep them unchanged and never regenerate for geometry drift.

## One-Shot Fuse

- one successful generated result per selected asset
- zero automatic content-repair generations
- zero cutout, Alpha, compositing, or registration attempts
- zero gray-to-green conversion or other background-replacement attempts
- tool or transport failure with no returned image does not consume the content
  attempt
- hard content failure produces a rejected or unresolved status
- a new generation requires a later explicit user request naming the asset

## Hard Content Failures

Reject when the first result contains:

- wrong identity, state, direction, or meaningful part count
- missing, duplicated, or invented owned parts
- foreign UI, text, icons, characters, badges, or decoration
- gray, white, checkerboard, transparent, textured, or nonuniform regions in the
  required green-screen background
- green spill, reflection, or transmitted green inside the intended subject
- background texture, cast shadow, floor, or scene material fused into the
  intended subject

## Evidence

Persist only concise evidence:

- source-reference path
- selected asset ID
- prompt or prompt summary
- first-result path
- native dimensions
- generation attempt count
- content-gate results
- terminal status and failure reason

Do not produce Alpha diagnostics, source overlays, difference heatmaps,
registration evidence, or multi-background cutout previews.
