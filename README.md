# Character Card V2: Explainer (WIP DRAFT)

- [./spec_v1.md](./spec_v1.md): Specification for "V1". V1 refers to the format currently in use across the character card ecosystem as of May 4th 2023. For reference, and for new implementers.
- [./spec_v2.md](./spec_v2.md): Specification for "V2". V2 adds new optional fields to V1 and is currently a work in progress.
- [./utility_code_snippets.md](./utility_code_snippets.md): Code snippets that may be individually referenced by the Explainer or the Specifications.
- [./keyword_definitions.md](./keyword_definitions.md): Definitions of MUST, SHOULD, and MAY as used in this repository

The V2 spec is fully backward compatible with V1, i.e. all V1 cards are
considered valid V2 cards.

Proposed new optional fields, reasons, details.

TODO: should more settings be a part of it?
TODO: add alternate_greetings

### spec

The value for this field should be `"chara_card_v2"`.

Ensuring character cards have a `spec` field will make it easier to parse cards if we create non-backward-compatible card formats in the future.

### creator_notes

A field where creators can include information that will never be included in the prompts.

Frontends **SHOULD** aim to make the information in this field as discoverable as possible so that botmakers can effectively communicate important usage notes, for example:

- "Bot optimized for GPT-4"
- "In your first response, introduce yourself"
- "High OAI temperature recommended (1.4)"

Screen estate for at least two paragraphs' worth of text should be reserved.

### system_prompt

TODO
TODO: should it be opt-in or opt-out?

### post_history_instructions

TODO: should it be opt-in or opt-out?
TODO: should it replace the frontend's ujb/jailbreak/postamble or supplement it?

### character_book

TODO

stacks with frontends' "world book"
