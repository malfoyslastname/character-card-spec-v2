# Character Card V2: Specification (WIP DRAFT)

This document describes new JSON fields added to the embedded JSON of V1
character cards. V1 refers to the format currently in use across the character
card ecosystem as of May 4th 2023.

[Character Card V1 Specification for new implementers](./spec_v1.md)
[Read the definitions of MUST, SHOULD, and MAY](./keyword_definitions.md)

[toc]

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
type TavernCardV1 = {
  name: string
  description: string
  personality: string
  scenario: string
  first_mes: string
  mes_example: string

  // New fields start here
  spec: 'chara_card_v2'
  creator_notes: string
  system_prompt: string
  post_history_instructions: string
  alternate_greetings: Array<string>
  character_book: CharacterBook
}

type CharacterBook = {
  // TBD
}
```

A frontend supporting both V1 and V2 would hence use a type looking like:

```ts
type TavernCard = TavernCardV1 | TavernCardV2
```

### spec

TODO

### creator_notes

TODO

### system_prompt

TODO after explainer

### post_history_instructions

TODO after explainer

### alternate_greetings

TODO

### character_book

TODO: find a format which is compatible with both Agnai and SillyTavern.
