# Character Card V1: Specification

This exists for reference in case of ambiguity, or for future new implementers.
[Read the definitions of MUST, SHOULD, and MAY](./keyword_definitions.md)

## Table of contents

- [Embedding methods](#embedding-methods)
- [Fields](#fields)
  * [name](#name)
  * [description](#description)
  * [personality](#personality)
  * [scenario](#scenario)
  * [first_mes](#first_mes)
  * [mes_example](#mes_example)

## Embedding methods

- .json file with no image. Discouraged for user-friendliness.
- PNG/APNG: JSON string encoded in base64 inside the "Chara" EXIF metadata field.
- WEBP: **Not covered by the spec due to technical ambiguities.**

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

All fields are mandatory and **MUST** default to the empty string, not null or absent/undefined.

In prompts sent to the AI, the fields `description`, `personality`, `scenario`, `first_mes`, and `mes_example` **MUST** replace the following magic strings, with a **case-insensitive** search (e.g. `<BOT>` and `<bot>` both work):
- {{char}} or `<BOT>` to the value of the card's `name` field
- {{user}} or `<USER>` to the application's set display name.

A default value for the user's name **MUST** exist.

Whether {{user}} and `<USER>` should be replaced inside the `name` field is an **UNSPECIFIED** edge case.

Details for each field follows.

### `name`

Used to identify a character.

### `description`

Description of the character.

**SHOULD** be included by default in every prompt.

Front-facing alternative names used by existing projects:

- ZoltanAI: "Personality"
- Agnai: "Persona Attributes" and "{{personality}}"
- Silly: "Description"

### `personality`

A short summary of the character's personality.

**SHOULD** be included by default in every prompt.

Front-facing alternative names used by existing projects:

- ZoltanAI: "Summary"
- Agnai: No name yet
- Silly: "Personality summary"

### `scenario`

The current context and circumstances to the conversation.

**SHOULD** be included by default in every prompt.

### `first_mes`

First message sent by the chatbot, also known as "greeting."

The chatbot **MUST** be the first one to send a message, and that message
**MUST** be the string inside `first_mes`.

### `mes_example`

Example conversations. It **MUST** be expected that botmakers format example
conversations like this:

```
<START>
{{user}}: hi
{{char}}: hello
<START>
{{user}}: hi
Haruhi: hello
```

`<START>` marks the beginning of a new conversation and **MAY** be transformed
(e.g. into an OpenAI System message saying "Start a new conversation.")

Example conversations **SHOULD**, by default, only be included in the prompt
until actual conversation fills up the context size, and then be pruned to make
room for actual conversation history. This behavior **MAY** be configurable by the user.
