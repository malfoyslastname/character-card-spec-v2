# Character Card V1: Specification (WIP DRAFT)

This exists for reference in case of ambiguity, or for future new implementers.
[Read the definitions of MUST, SHOULD, and MAY](./keyword_definitions.md)

[toc]

## Embedding method

TODO: complete and properly write shis section

- json string encoded in base64 inside the "Chara" EXIF metadata field
- can also be a normal json but this is discouraged for user-friendliness
- different for webp and png (TODO)
[Code examples: reading a character card file](./utility_code_snippets.md#reading_a_character_card)
[Code examples: writing a character card file](./utility_code_snippets.md#writing_a_character_card)

## Fields

The current format can be represented as this TypeScript type:

```ts
type TavernCard = {
  name: string
  description: string
  personality: string
  scenario: string
  first_mes: string
  mes_example: string
}
```

All fields are mandatory and **MUST default to the empty string, not undefined or null** (TODO: confirm this)

In prompts sent to the AI, all fields except `name` **MUST** replace the following magic strings, with a **case-insensitive** search (e.g. <BOT> and <bot> both work):
- {{char}} or <BOT> to the value of the card's `name` field
- {{user}} or <USER> to the application's set display name.

A default value for the user's name **MUST** exist.

Whether {{user}} and <USER> should be replaced inside the `name` field is an **UNSPECIFIED** edge case. (TODO: See if existing implementations all agree on this, then if yes, make that officially specified)

Details for each field follows.

### name

Used to identify a character.

In **all other fields**, the magic strings `{{char}}` and `<BOT>` (case-insensitive) **MUST** get replaced by this value.

### description

### personality

### scenario

### first_mes

### mes_example
