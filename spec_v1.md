# Character Card V1: Specification (WIP DRAFT)

This exists for reference in case of ambiguity, or for future new implementers.

[toc]


## Embedding method (no changes from V1)

TODO: complete and properly write shis section

- json string encoded in base64 inside the "Chara" EXIF metadata field
- different for webp and png
[Code examples: reading a character card file](./utility_code_snippets.md#reading_a_character_card)
[Code examples: writing a character card file](./utility_code_snippets.md#writing_a_character_card)

## Existing fields (skip this if you've already implemented V1)

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

### name

### description

### personality

### scenario

### first_mes

### mes_example
