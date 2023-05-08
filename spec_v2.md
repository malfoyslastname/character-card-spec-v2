# Character Card V2: Specification (WIP DRAFT)

This document describes new JSON fields added to the embedded JSON of V1
character cards. V1 refers to the format currently in use across the character
card ecosystem as of May 4th 2023.

[Character Card V1 Specification for new implementers](./spec_v1.md)

[Read the definitions of MUST, SHOULD, and MAY](./keyword_definitions.md)

## Table of contents

- [New fields](#new-fields)
  * [`spec`](#spec)
  * [`spec_version`](#spec_version)
  * [creator_notes](#creator_notes)
  * [system_prompt](#system_prompt)
  * [post_history_instructions](#post_history_instructions)
  * [alternate_greetings](#alternate_greetings)
  * [character_book](#character_book)
  * [tags](#tags)
  * [creator](#creator)
  * [character_version](#character_version)
  * [extensions](#extensions)

## New fields

The V1 (current) spec can be described with this TypeScript definition:

```ts
type TavernCardV1 = {
  name: string
  description: string
  personality: string
  scenario: string
  first_mes: string
  mes_example: string
}
```

The V2 spec can be described with:

```ts
type TavernCardV2 = {
  spec: 'chara_card_v2'
  spec_version: '2.0' // May 8th addition
  data: {
    name: string
    description: string
    personality: string
    scenario: string
    first_mes: string
    mes_example: string

    // New fields start here
    creator_notes: string
    system_prompt: string
    post_history_instructions: string
    alternate_greetings: Array<string>
    character_book?: CharacterBook

    // May 8th additions
    tags: Array<string>
    creator: string
    character_version: number
    extensions: Record<string, any>
  }
}

type CharacterBook = {
  // TBD
}
```

A frontend supporting both V1 and V2 would hence use a type looking like:

```ts
type TavernCard = TavernCardV1 | TavernCardV2
```

If you're unsure what this means in plain JavaScript terms, check out [the "Proposed fields" section introduction of the explainer](./README.md#proposed_fields)

**All V1 fields follow [the V1 spec](./spec_v1.md), other than being nested
inside the `data` property.**

### `spec`

The value for this field **MUST** be `"chara_card_v2"`.

### `spec_version`

The value for this field **MUST** be `"2.0"`.

### `creator_notes`

The value for this field **MUST NOT** be used inside prompts. The value for this field **SHOULD** be very discoverable for bot users (at least one paragraph **SHOULD** be displayed).

### `system_prompt`

Frontends' default behavior **MUST** be to replace what users understand to be the "system prompt" global setting with the value inside this field. (Exception: if the field value is an empty string, the user's "system prompt" setting or an internal fallback **MUST** be used.)

Frontends **MAY** offer ways to replace or supplement character cards' system prompt (in addition to directly editing the card), but it **MUST NOT** be the default behavior.

### `post_history_instructions`

Frontends' default behavior **MUST** be to replace what users understand to be the "ujb/jailbreak" setting with the value inside this field. (Exception: if the field value is an empty string, the user's "system prompt" setting or an internal fallback **MUST** be used.)

Frontends **MAY** offer ways to replace or supplement character cards' system prompt (in addition to directly editing the card), but it **MUST NOT** be the default behavior.

### `alternate_greetings`

Array of strings.

Frontends **MUST** offer "swipes" on character first messages, each string inside this array being an additional "swipe".

### `character_book`

A character-specific lorebook. **The lorebook format is not yet defined, but will be prior to official adoption of the spec.**

Frontends **MUST** use the character lorebook by default.

Character editors **MUST** save character lorebooks in the specified format.
(**Lorebook format not yet defined, but will be before official adoption.**)

Character lorebook **SHOULD** stack with user "world book"/"world info"/"memory book". (Character book SHOULD take full precedence over world book.)

### `tags`

An array of strings.

There is no restriction on what strings are valid.

This field **SHOULD NOT** be used in the prompt engineering.

This field **MAY** be used for frontend sorting/filtering purposes (SHOULD be
case-insensitive).

### `creator`

This field **MUST NOT** be used for prompt engineering.

This field **MAY** be shown on frontends.

### `character_version`

This field **MUST NOT** be used for prompt engineering.

This field **MAY** be shown on frontends and used for sorting.

### `extensions`

This field **MUST** default to an empty object (`{}`).

This field **MAY** contain any arbitrary JSON key-value pair.

Character editors **MUST NOT** destroy unknown key-value pairs when importing and exporting character cards.

Applications **MAY** store any unspecified data in this object.

Applications **SHOULD** namespace any key they use to prevent conflicts, e.g.
`"agnai/voice": /* ... */` or `"agnai_voice": /* ... */` or `"agnai": { "voice": /* ... */ }"`.
