---
name: kanon-wisdom
description: >-
  Route biblical text queries to the Wisdom and Poetic Books - Job, Psalms, Proverbs,
  Ecclesiastes, Song of Solomon.
  Use when a query names one of these five books, or asks about the fear of the Lord,
  the suffering of Job, theodicy, comfort in grief, the Psalms, lament or imprecatory
  psalms, the Shepherd Psalm, praise poetry, the proverbs of Solomon, Lady Wisdom,
  the meaning of life and vanity of vanities, or the allegory of married love.
  Also triggers on "wisdom literature", "poetic books", "Psalter", "Song of Songs",
  "Canticles", "Qoheleth".
---

# kanon-wisdom

Retrieve text from the five wisdom and poetic books. This skill owns the routing decision
and the parameter contract. It does not own translation choice or the text store.

## Books Owned

Job, Psalms, Proverbs, Ecclesiastes, Song of Solomon.

Chapter ranges: Job 1-42, Psalms 1-150, Proverbs 1-31, Ecclesiastes 1-12, Song of
Solomon 1-8. Psalms is the only book here with a three-digit chapter range. Do not apply
a 66-chapter ceiling to it.

## Route Here When

- The query names one of the five books above.
- The query names a person or speaker whose home is these books: Job, Eliphaz, Bildad,
  Zophar, Elihu, the psalmists, David as psalm author, Asaph, the sons of Korah, Solomon
  as the speaker of Proverbs or Ecclesiastes, Agur, King Lemuel.
- The query names a theme from these books: the wager in heaven, the whirlwind speech,
  "I know that my redeemer lives", the fear of the Lord as the beginning of wisdom,
  the two paths, the virtuous woman, the Shepherd Psalm, the valley of the shadow,
  imprecation, lament, selah, the vanity of vanities, "a time to be born", "under the
  sun", the garden imagery of married love.
- The query names a psalm by number or by title, for example "Psalm 51" or the
  "Miserere". Route by the psalm number as the chapter with `book` set to Psalms.

## Do Not Route Here

| Query looks like | Actual owner |
|---|---|
| Genesis through Deuteronomy | `kanon-law` |
| Joshua through Esther | `kanon-history` |
| Isaiah through Malachi, including Lamentations | `kanon-prophets` |
| Matthew through Revelation | `kanon-new-testament` |

Three traps. "Song of Solomon" is a wisdom book, not a Kings passage. Lamentations and
its laments belong to `kanon-prophets` even though they read like poetry. The wisdom of
Solomon as a story about the man belongs to `kanon-history`, while the collected sayings
belong here. Use the cited book, not the genre, to decide.

## Parameter Resolution

Emit exactly three parameters: `book`, `chapter`, `query`. The schema rejects anything
else, so do not add translation, verse, or range fields.

- `book` - normalize to an enum value from `schema.json`. Use "Psalms" even when the user
  says "Psalm". Use "Song of Solomon" for "Song of Songs" and "Canticles". Use
  "Ecclesiastes" for "Qoheleth" and "the Preacher".
- `chapter` - emit a bare integer. A standalone psalm number is the chapter: "Psalm 119"
  gives `book` Psalms and `chapter` 119. Convert spelled-out numbers and ordinals.
  A quoted verse with no reference gets asked for once, not guessed, because these books
  repeat their vocabulary heavily.
- `query` - the search intent in the user's own topic words with book and chapter
  removed. Keep poetic phrasing and the emotional register, which carry retrieval signal
  here more than in narrative books. Do not answer in the `query` field.

## Output Contract

Return the retrieved passage alongside the normalized reference. If a chapter is out of
range, say so and offer the nearest valid chapter. Do not silently substitute. Note that
some psalm traditions number psalms differently, so state the numbering used when the
retrieved text suggests a discrepancy.
