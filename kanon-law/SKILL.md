---
name: kanon-law
description: >-
  Route biblical text queries to the Law (Torah, Pentateuch) - Genesis, Exodus,
  Leviticus, Numbers, Deuteronomy.
  Use when a query names one of these five books, or asks about creation, the patriarchs,
  the exodus, the plagues, Mount Sinai, the Ten Commandments, the tabernacle,
  the sacrificial system, the Levitical priesthood, purity law, the Day of Atonement,
  the wilderness wanderings, Balaam, the Shema, the covenant blessings and curses,
  or the death of Moses.
  Also triggers on "Torah", "Pentateuch", "five books of Moses", "law of Moses".
---

# kanon-law

Retrieve text from the five books of the Law. This skill owns the routing decision and
the parameter contract. It does not own translation choice or the text store.

## Books Owned

Genesis, Exodus, Leviticus, Numbers, Deuteronomy.

Chapter ranges: Genesis 1-50, Exodus 1-40, Leviticus 1-27, Numbers 1-36, Deuteronomy 1-34.

## Route Here When

- The query names one of the five books above.
- The query names a person or event whose canonical home is these books: Adam, Noah,
  Abraham, Sarah, Isaac, Rebekah, Jacob, Rachel, Joseph, Moses, Aaron, Miriam, Pharaoh,
  Balaam, Joshua as Moses' aide.
- The query names a theme from these books: creation, the fall, the flood, the tower of
  Babel, circumcision, the burning bush, the ten plagues, Passover, the Red Sea,
  manna, water from the rock, the golden calf, the tabernacle, the ark of the covenant,
  the mercy seat, the burnt, grain, peace, sin and guilt offerings, clean and unclean,
  leprosy regulation, the scapegoat, the jubilee, the Nazirite vow, the census, the
  spies, Korah's rebellion, the bronze serpent, the cities of refuge, the Shema,
  "a land flowing with milk and honey".

## Do Not Route Here

| Query looks like | Actual owner |
|---|---|
| The conquest of Canaan, Jericho, the judges, David, Solomon, the exile | `kanon-history` |
| Job, Psalms, Proverbs, Ecclesiastes, Song of Solomon | `kanon-wisdom` |
| Isaiah through Malachi, including Lamentations and Daniel | `kanon-prophets` |
| Matthew through Revelation | `kanon-new-testament` |
| A quote of Moses recorded in a later book | Confirm the cited book |

Joshua is the boundary case. Joshua the man appears in Exodus and Numbers, so those
passages stay here. The book of Joshua starts `kanon-history`.

## Parameter Resolution

Emit exactly three parameters: `book`, `chapter`, `query`. The schema rejects anything
else, so do not add translation, verse, or range fields.

- `book` - normalize the user's wording to an enum value from `schema.json`. Accept and
  repair "the first book of Moses" to Genesis, "Deut." to Deuteronomy, "Numbers chapter
  four" to Numbers.
- `chapter` - emit a bare integer. Convert ordinals and spelled-out numbers ("chapter
  twelve" becomes 12). If the user gives a verse but no chapter, infer the chapter from
  context. If the reference is unresolved, ask once rather than guessing a chapter.
- `query` - write the search intent in the user's own topic words with the book and
  chapter removed. Keep synonyms and doctrinal phrasing. Do not answer the question in
  the `query` field.

When the user cites a range spanning books, for example Genesis 50 to Exodus 1, split
it into one tool call per book. Never broaden a single call to cover both.

## Output Contract

Return the retrieved passage alongside the normalized reference, so the caller can see
what was actually looked up. If a chapter falls outside the range listed above, say so
and offer the nearest valid chapter. Do not silently substitute a passage.
