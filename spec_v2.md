# Character Card V2: Specification (WIP DRAFT)

This document describes new JSON fields added to the embedded JSON of V1
character cards. V1 refers to the format currently in use across the character
card ecosystem as of May 4th 2023.

[Character Card V1 Specification for new implementers](./spec_v1.md)

[toc]

## Keywords

- **MUST**: Adoption of this spec requires adhering to this requirement.
- **SHOULD**/**SHOULD NOT**: There may exist valid reasons in particular circumstances to ignore this requirement, but the full implications mut be understood and carefully weighed.
- **MAY**: A truly optional requirement.
- **UNSPECIFIED**: Legacy unclear behavior due to lack of existing ecosystem consensus in v1 implementations, or due to lack of foresight when writing v2. Ideally, this should not exist.

## New fields

All those files are **optional** for backward-compatibility (that is to say, all
V1). **Optional** means that those fields are **allowed to be absent from the
JSON object, or `null`**.

FIELD NAMES ARE VERY EARLY TENTATIVE

The full updated V2 spec can be represented by this Typescript type:

```ts
type TavernCard = {
  name: string
  description: string
  personality: string
  scenario: string
  first_mes: string
  mes_example: string

  // New fields start here
  system_prompt?: string
  ujb?: string
  character_book: CharacterBook
}

type CharacterBook = {
  // TBD
}
```


### system_prompt

TODO after explainer

### ujb

TODO after explainer

### character_book

TODO: find a format which is compatible with both Agnai and SillyTavern.
