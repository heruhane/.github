---
id: DEC-14
title: "The Japanese language content standard: a native speaker checks what learners read"
date: 2026-10-30
topics: content
holding: "Japanese that learners read is checked by a native speaker, and a project names who has the final word on its language content. Anything that scores, marks or explains Japanese has known answers checked by a language expert. Japanese is marked with lang=ja, stays real text, is not sent with a whole font, and is compared and counted deliberately."
amended_by:
superseded_by:
amends:
supersedes:
applies:
codified_in: STANDARDS.md, Japanese language content standard
---

## 14. The Japanese language content standard: a native speaker checks what learners read

*Decided 30 October 2026, in standards version 2026-10-30.1.*

**Decided.** Japanese that learners read is checked by a native speaker, and a project names who has the final word on its language content. Anything that scores, marks or explains Japanese has a fixture set of known answers checked by a language expert. Japanese is marked with `lang="ja"`, stays real text with furigana in `<ruby>`, is not sent with a whole font, and is compared and counted deliberately.

heruhane teaches Japanese, so a mistake in it is the most serious kind of bug: a learner cannot tell it from the truth. Whether a sentence is natural is a judgement that automated checks cannot make. The technical rules exist because Japanese text fails in ways that English text does not. Without `lang="ja"`, a browser may draw the text with Chinese forms of the characters. A complete Japanese font runs to several megabytes. Unicode allows more than one encoding of the same kana, and words have no spaces between them.

Two alternatives were considered. **Relying on automated grammar or readability checks** was rejected, for the reason above, and readability scores built for English do not apply to Japanese. **Self-hosting a complete font**, so that every reader sees the same glyphs, was rejected because of what it costs a learner on a slow connection.

**Obligations.**

- **Follow the [Japanese language content standard](../STANDARDS.md#japanese-language-content-standard)** in any repository that shows, teaches or processes Japanese.
- **Record what has been checked**, so that unchecked content is not mistaken for finished work.
