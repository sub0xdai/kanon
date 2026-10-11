---
name: kanon-novum-testamentum
description: >-
  Route biblical text queries to the New Testament - Matthew, Mark, Luke, John, Acts,
  Romans, 1 and 2 Corinthians, Galatians, Ephesians, Philippians, Colossians,
  1 and 2 Thessalonians, 1 and 2 Timothy, Titus, Philemon, Hebrews, James, 1 and 2 Peter,
  1, 2 and 3 John, Jude, Revelation.
  Use when a query names one of these twenty-seven books, or asks about the birth,
  baptism, miracles, parables, passion, crucifixion, resurrection or ascension of Jesus,
  the Sermon on the Mount, the Beatitudes, the Lord's Prayer, the Last Supper, Pentecost,
  the conversion of Paul, the Jerusalem council, justification by faith, grace,
  the fruit of the Spirit, spiritual gifts, the armor of God, the kenosis hymn,
  the return of Christ, the millennium, the lake of fire, or the new Jerusalem.
  Also triggers on "gospels", "epistles", "Pauline letters", "synoptic", "apocalypse".
---

# kanon-novum-testamentum

Retrieve text from the twenty-seven New Testament books. This skill owns the routing
decision and the parameter contract. It does not own translation choice or text store.

## Books Owned

Gospels and Acts: Matthew, Mark, Luke, John, Acts.
Pauline letters: Romans, 1 Corinthians, 2 Corinthians, Galatians, Ephesians,
Philippians, Colossians, 1 Thessalonians, 2 Thessalonians, 1 Timothy, 2 Timothy,
Titus, Philemon.
General letters: Hebrews, James, 1 Peter, 2 Peter, 1 John, 2 John, 3 John, Jude.
Apocalypse: Revelation.

Chapter ranges span Philemon, 2 John, 3 John and Jude at a single chapter up to Acts 28
and Revelation 22. Matthew has 28, Mark 16, Luke 24, John 21, Romans 16, 1 Corinthians
16, Hebrews 13. Mark's traditional ending and the longer ending of Mark differ across
manuscripts, so confirm before serving Mark 16.

## Route Here When

- The query names one of the twenty-seven books above.
- The query names a person whose home is these books: Jesus, Mary, Joseph, John the
  Baptist, Herod, the twelve disciples, Peter, Andrew, James, John, Matthew, Thomas,
  Judas Iscariot, Mary Magdalene, Martha, Lazarus, Zacchaeus, Nicodemus, Pilate,
  Barabbas, Paul, Barnabas, Silas, Timothy, Titus, Priscilla, Aquila, Apollos,
  Stephen, Ananias, Sapphira, Lydia, Philemon, Onesimus, Theophilus.
- The query names a theme from these books: the virgin birth, the magi, the flight to
  Egypt, the temptations, the transfiguration, the feeding of the five thousand, the
  walking on water, the raising of Lazarus, the triumphant entry, Gethsemane, the
  empty tomb, the road to Emmaus, the doubting of Thomas, the great commission,
  the day of Pentecost, the Damascus road, the Areopagus, the fruit of the Spirit,
  the rapture, the great white throne, the marriage supper of the Lamb.

## Do Not Route Here

| Query looks like | Actual owner |
|---|---|
| Genesis through Deuteronomy | `kanon-lex` |
| Joshua through Esther | `kanon-historia` |
| Job through Song of Solomon | `kanon-sapientia` |
| Isaiah through Malachi | `kanon-prophetae` |

Name traps. "Hebrews" is a New Testament letter despite sounding Old Testament.
Revelation is this division, and a query about the apocalypse belongs here unless it
cites Daniel, Zechariah, or Isaiah, in which case route by the cited book. Old Testament
quotations inside New Testament books stay with the New Testament book that quotes them:
Matthew citing Isaiah is a Matthew query.

## Parameter Resolution

Emit exactly three parameters: `book`, `chapter`, `query`. The schema rejects anything
else, so do not add translation, verse, or range fields.

- `book` - normalize to an enum value from `schema.json`. Numeric names keep the numeral
  and one space: "1 Corinthians", not "First Corinthians" or "I Cor". Use "Revelation",
  not "Revelations" or "Apocalypse". Repair "Mt", "Mk", "Lk", "Jn", "Rom", "Gal",
  "Eph", "Phil", "Col", "Heb", "Rev".
- `chapter` - emit a bare integer. Convert ordinals and spelled-out numbers. Single
  chapter books take 1. If the user gives a verse with no chapter, infer the chapter from
  context, and if it stays ambiguous, ask once rather than guessing.
- `query` - the search intent in the user's own topic words with book and chapter
  removed. Keep doctrinal terminology. Do not answer in the `query` field.

A citation spanning books, for example John 21 to Acts 1, becomes one call per book.

## Output Contract

Return the retrieved passage alongside the normalized reference. If a chapter is out of
range, say so and offer the nearest valid chapter. Do not silently substitute. For
passages with known manuscript variation, state the reading served.
