# Character Card V2: Explainer (First draft)

## Repository map

- [./README.md](./README.md): This document. An explanation of what V2 proposes,
    including justification for some of its choices. Completely up for changes
    depending on the feedback of developers and botmakers in the ecosystem.
- [./spec_v1.md](./spec_v1.md): Specification for "V1". V1 refers to the format currently in use across the character card ecosystem as of May 4th 2023. For reference, and for new implementers.
- [./spec_v2.md](./spec_v2.md): Specification for "V2". **Not yet written.**
- [./utility_code_snippets.md](./utility_code_snippets.md): Code snippets that may be individually referenced by the Explainer or the Specifications.
- [./keyword_definitions.md](./keyword_definitions.md): Definitions of MUST, SHOULD, and MAY as used in the specifications.
- [General discussion & summary of project owner positions](https://github.com/malfoyslastname/character-card-spec-v2/issues/1)

## Table of contents

- [Introduction](#introduction)
- [Proposed fields](#proposed_fields)
  * [`spec`](#spec)
  * [`creator_notes`](#creator_notes)
  * [`system_prompt`](#system_prompt)
  * [`post_history_instructions`](#post_history_instructions)
  * [`alternate_greetings`](#alternate_greetings)
  * [`character_book`](#character_book)

## Introduction

The Character Card V2 Specification is a proposal for a new format for character cards containing new fields. Briefly, the aims of this V2 spec are:

- With the fields `system_prompt` and `post_history_instructions`, to give botmakers control over the experience that users have with their bots. Some of this control was first lost when abandoning Character.AI, and then further lost when the "UJB/Jailbreak" tech was discovered. It is a source of frustration for both botmakers who don't know what kind of experience their users are having, and for users who have to fiddle with very sensitive and frustrating settings.
- With the field `character_book`, to make it more convenient to attach a lorebook to a character, so users don't have to download and import two separate files. This creates a new "World book" vs "Character book" distinction.
- With the field `creator_notes`, to make it easier for botmakers to communicate important information to users.
- With the field `alternate_greetings`, to add a new useful feature to the character cards. If you have other new features to add, let's discuss them.

Details for each field are included below.

## Proposed fields

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

What this means in plain JavaScript terms is that given a card named `chara`:

- if `(chara.spec === undefined)`, then you can be sure that the card contains nothing but `chara.name`, `chara.description`, `chara.personality`, `chara.scenario`, `chara.first_mes`, `chara.mes_example`.
- if `(chara.spec === 'chara_card_v2')`, then you can be sure that `chara.data.name`, `chara.data.description`, `chara.data.personality`, `chara.data.scenario`, `chara.data.first_mes`, `chara.data.mes_example`, `chara.data.creator_notes`, `chara.data.system_prompt`, `chara.data.post_history_instructions` and `chara.data.alternate_greetings` are all present (neither null, undefined, nor absent). `chara.data.character_book` may still be absent.

The reason why old and new fields are nested into a `data` object in the V2 spec is to prevent V1-only editors (e.g. ZoltanAI, if the author cannot be reached) from successfully loading a V2 card, but discarding its V2-only fields.

### `spec`

The value for this field should be `"chara_card_v2"`.

Ensuring character cards have a `spec` field will make it easier to create more card formats in the future, if needed.

### `creator_notes`

A field where creators can include information that will **never** be included in the prompts.

Frontends should aim to make the information in this field as discoverable as possible so that botmakers can effectively communicate important usage notes, for example:

- "Bot optimized for GPT-4"
- "In your first response, introduce yourself"
- "High OAI temperature recommended (1.4)"

Screen estate for at least one paragraph' worth of text should be reserved somewhere discoverable. The existing field on characterhub.org named "Tagline" could map to `creator_notes`.

### `system_prompt`

Up until now, system prompt was set by the user in the frontend settings. An example of common system prompt wording might be:

> Write {{char}}'s next reply in a fictional chat between {{char}} and {{user}}. Write 1 reply only in internet RP style, italicize actions, and avoid quotation marks. Use markdown. Be proactive, creative, and drive the plot and conversation forward. Write at least 1 paragraph, up to 4. Always stay in character and avoid repetition.

The Character Card V2 project proposes that cards may contain their own system
prompt, **meant to completely replace the system prompt set by the user**.

In order to give back to botmakers control over the experience users have with their bots, this spec **requires** that **the default behavior be for this new `system_prompt` to override the user's global system prompt, except if `system_prompt` is an empty string.** (In the case of an empty string, frontends should fall back to the user's global system prompt or to an internal default.)

Frontends may still add a setting for the user's global system prompt to
override this field, but it should not be the default. Alternatively, users can
simply edit the card if they aren't satisfied with the botmaker's system prompt.

Frontends may, if they want, create a setting for a text that is appended or prepended to the character system prompts, but **this setting should be different from what is currently understood as the user system prompt**.

### `post_history_instructions`

It was discovered recently (months later than the invention of Tavern cards) that system instructions written *after* the conversation history have a much stronger weight on current models' generations than instructions written *before* the conversation history (specifically, the system prompt and [the `description` field](./spec_v1.md#description)).

This has been implemented for users to exploit in certain frontends as a user
setting. In Agnai, it's called "UJB" (ultimate jailbreak). In Silly, it's called
"Jailbreak".

The Character Card V2 project proposes that botmakers should be able to choose
to handle this for the user, if desired. The variety of post-history instructions users are employing makes it very difficult for a botmaker to predict how their bot will behave for others, and indeed, often causes users to have poorer experiences than the botmakers intended.

In order to give back to botmakers control over the experience users have with their bots, this spec **requires** that **the default behavior be for this new `post_history_instructions` to override what is currently named the Agnai UJB and the Silly Jailbreak, except if `post_history_instructions` is an empty string.** (In the case of an empty string, frontends should fall back to the user's global UJB/Jailbreak or to an internal default.)

Frontends may still add a setting for the user's global system prompt to
override this field, but it should not be the default. Alternatively, users can
simply edit the card if they aren't satisfied with the botmaker's post-history
instructions.

Frontends may, if they want, create a setting for a text that is appended or prepended to the character post-history instructions, but **this setting should be different from what is currently understood as the UJB/Jailbreak**.

### `alternate_greetings`

This field would be an array of strings, each representing an additional alternate greeting to the one stored in [the `first_mes` field](./spec_v1.md#first-mes). This would allow botmakers to offer different starting scenarios for the same character.

The expected UX for this field is similar to the current "swiping" mechanisms Agnai and Silly have implemented for non-greeting bot messages.

### `character_book`

First, let's define some terms we'll use:

- **Lorebook**: An object containing entries which are included in the prompt when
    specified keywords are found in the conversation history. Term coined by
    Novel.AI. Named Memory Book in Agnai and World Info in Silly.
- **World book**: A *kind* of lorebook which is not tied to any character card,
    and applies to all of the user's chat. This is also what Agnai's Memory Book
    and Silly's World Info are.
- **Character book**: A proposed new *kind* of lorebook which would be embedded
    into character cards, and only be applied for that character's chats.

It has become very common for character cards to be meant to be used in
combination with a specific World book. However, botmakers are not confident
that users are properly getting their recommended world books, and the extra
effort required for users to download and import external worldbooks turns them
away from using this kind of character card.

Character books must stack with world books, not replace them. If a user has a world book with information about fruits, and a character card has a world book with information about (**Needs confirmation**. While this is the ideal scenario, maintainers should confirm whether there is a reliable way to combine two lorebooks which may have different weighing scales.)

**The format used will be neither Agnai's nor Silly's**, but should be compatible with the current features of both. Frontends may choose not to ignore fields they don't support, but are encouraged try to adapt them to their own system. Code examples for this will be provided in a future draft of this repository.

The Character Card V2 spec needs to use a different format for Character Books
than Agnai's and Silly's, because their format might change over time. We do not
want other frontends to have to keep up with the format changes of other
frontends, so there should be an immutable spec for character cards.
