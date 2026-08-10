# Manifest Eligibility Filter

Apply this filter before component-tree construction, asset numbering, layer
counting, or user confirmation.

## Hard Exclusion: System Chrome

Exclude the entire operating-system and device-chrome family:

- time and date
- signal bars and carrier or network label
- Wi-Fi, Bluetooth, location, navigation, recording, and privacy indicators
- battery shell, level, and charging mark
- system gesture bar and Home Indicator

Exclude both text and graphical indicators. Never create a "system interface"
section in the user-visible component tree. Never list these elements as
optional, disabled, skipped, or not exported.

## Hard Exclusion: Ordinary Default Text

Exclude ordinary interface and content copy:

- subtitle, slogan, explanation, instruction, and activity notice
- navigation, tab, menu, rule, share, record, and shortcut copy
- button, label, badge, task, tooltip, and toast copy
- values, counters, prices, dates, times, percentages, and progress text
- product name, duration, category name, and card caption

An independently useful non-text container may remain as a blank asset. Rebuild
the complete owned surface when necessary, but do not create a record for the
removed text.

## Single Exception: Top-KV Main-Title Group

Allow at most one primary main-title visual group when it is:

- located in the top hero or KV region
- the dominant campaign or page title
- visually styled as a title rather than ordinary UI copy

Treat the group as one indivisible processing and delivery asset. Include:

- title glyphs
- outline, shadow, glow, extrusion, and inner texture
- attached or dependent backplate
- inseparable particles and highlights
- integrated edition or issue mark
- ornaments that cannot stand as independent UI assets

Exclude nearby subtitles, dates, notices, activity times, navigation text, and
independent promotional copy even when they share the same region.

If no clear dominant top-KV title exists, allow no text asset. If two candidates
compete, choose the visually dominant primary title; do not include both.

## Embedded Identity Text

Keep lettering embedded in a logo, product, packaging, illustrated badge,
mascot prop, or other compact identity asset with that graphic when separating
it would damage identity. Do not create an independent text asset.

## User-Visible Manifest Invariant

The final confirmation list must contain only eligible assets. Excluded system
and default-text elements must not appear in:

- component tree
- asset IDs
- atomic or delivery counts
- manifest records
- merge proposals
- unresolved or rejected lists
- delivery filenames

The user must never need to remove these elements manually.
