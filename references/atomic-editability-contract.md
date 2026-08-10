# Atomic Editability And User Grouping Contract

Apply `manifest-eligibility-filter.md` before this contract.

## Atomic Processing Rule

Create a separate proposed asset when any answer is yes:

| Question | Separate when |
|---|---|
| Can it move, hide, or appear alone? | yes |
| Can its state or content change alone? | yes |
| Does it have its own interaction target or z-order? | yes |
| Is it a meaningful repeated sibling? | yes |
| Would a UI designer reasonably select it alone? | yes |

Stop at the smallest independently meaningful object, not the smallest drawable
primitive. Keep the intrinsic parts of logos, mascots, products, reward art,
packages, and illustrated identities together.

## User Grouping Override

Treat explicit user instructions to combine, exclude, rename, or reorder assets
as authoritative for the current manifest. A user-approved group becomes one
delivery asset and one ImageGen request.

Record:

- `delivery_group_id`
- every member and expected owned part
- the user's grouping instruction or manifest justification
- the complete `must_exclude` list

Do not silently split an approved group during generation.

## Repeated Instances

Give repeated instances separate IDs only when independent editability is
desired. Otherwise allow a user-approved combined set, such as a complete group
of reward bubbles, to remain one asset.

Never reuse one generated raster as an unrequested sibling asset.

## Structural Rejection

Reject when:

- an output includes unapproved independent controls
- a requested member of an approved group is missing
- an unrequested sibling is added
- a compact identity asset is fragmented internally
- the generated output no longer matches the confirmed manifest ownership
