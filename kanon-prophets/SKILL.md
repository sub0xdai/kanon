---
name: kanon-prophets
description: >-
  Route biblical text queries to the Prophetic Books - Isaiah, Jeremiah, Lamentations,
  Ezekiel, Daniel, Hosea, Joel, Amos, Obadiah, Jonah, Micah, Nahum, Habakkuk,
  Zephaniah, Haggai, Zechariah, Malachi.
  Use when a query names one of these seventeen books, or asks about prophetic judgment
  and restoration, the suffering servant, Emmanuel, the new covenant, the valley of dry
  bones, the fiery furnace, the lion's den, Daniel's visions, the seventy weeks,
  the day of the Lord, the locusts, justice and Amos, Jonah and the great fish, Nineveh,
  Bethlehem Ephrathah, or the messenger of Malachi.
  Also triggers on "major prophets", "minor prophets", "latter prophets",
  "writing prophets", "apocalyptic".
---

# kanon-prophets

Retrieve text from the seventeen prophetic books. This skill owns the routing decision and
the parameter contract. It does not own translation choice or the text store.

## Books Owned

Isaiah, Jeremiah, Lamentations, Ezekiel, Daniel, Hosea, Joel, Amos, Obadiah, Jonah,
Micah, Nahum, Habakkuk, Zephaniah, Haggai, Zechariah, Malachi.

Chapter ranges span Obadiah, a single chapter, up to Isaiah 66. Jeremiah has 52,
Lamentations 5, Ezekiel 48, Daniel 12, Hosea 14, Joel 3, Amos 9, Jonah 4, Micah 7,
Nahum 3, Habakkuk 3, Zephaniah 3, Haggai 2, Zechariah 14, Malachi 4. Joel's length
differs between the Hebrew and Greek traditions, so confirm chapter 3 before serving it.

## Route Here When

- The query names one of the seventeen books above.
- The query names a prophet whose book is here: Isaiah, Jeremiah, Ezekiel, Daniel,
  Hosea, Joel, Amos, Obadiah, Jonah, Micah, Nahum, Habakkuk, Zephaniah, Haggai,
  Zechariah, Malachi. Also the wife of Hosea, Gomer.
- The query names a theme from these books: the call vision and the coal on the lips,
  the servant songs, "a virgin shall conceive", the prince of peace, the wolf and the
  lamb, the potter's house, the new covenant written on hearts, the seventy years,
  the valley of dry bones, the four living creatures, the statue with feet of clay,
  the handwriting on the wall, the seventy weeks, the fourth beast, the day of the Lord,
  "let justice roll down like waters", the great fish, "out of Egypt I called my son",
  "the just shall live by faith", the flying scroll, "behold, I send my messenger".
- The query names an image the book supplies to later texts, for example the lion and
  the lamb of Isaiah 11 or the son of man of Daniel 7. Use the cited prophetic book, not
  the later book that quotes it.

## Do Not Route Here

| Query looks like | Actual owner |
|---|---|
| Genesis through Deuteronomy | `kanon-law` |
| Joshua through Esther, where the prophets appear as characters | `kanon-history` |
| Job through Song of Solomon | `kanon-wisdom` |
| Matthew through Revelation | `kanon-new-testament` |

Two canon traps. Daniel and Lamentations belong to this division in this ordering, even
though the Hebrew canon files them under the Writings. And a prophet as a *character* in
narrative stays with the narrative: Elijah in 1 Kings is `kanon-history`, while Zechariah
son of Jehoiada in 2 Chronicles is `kanon-history` too, not this skill. Route by cited
book.

## Parameter Resolution

Emit exactly three parameters: `book`, `chapter`, `query`. The schema rejects anything
else, so do not add translation, verse, or range fields.

- `book` - normalize to an enum value from `schema.json`. Repair "Isa", "Jer", "Lam",
  "Ezek", "Dan", "Zech", "Mal". Note that many prophetic books are named after figures
  who appear elsewhere, so confirm the user means the book.
- `chapter` - emit a bare integer. Convert ordinals and spelled-out numbers. Single
  chapter books such as Obadiah take 1. If the user gives a verse with no chapter, infer
  the chapter from context, and if it stays ambiguous, ask once rather than guessing.
- `query` - the search intent in the user's own topic words with book and chapter
  removed. Keep symbolic and apocalyptic wording, which carries retrieval signal. Do not
  answer in the `query` field.

## Output Contract

Return the retrieved passage alongside the normalized reference. If a chapter is out of
range, say so and offer the nearest valid chapter. Do not silently substitute. For books
whose chapter divisions differ across traditions, state the numbering used.
