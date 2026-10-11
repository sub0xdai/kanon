---
name: kanon-history
description: >-
  Route biblical text queries to the Historical Books - Joshua, Judges, Ruth, 1 and 2
  Samuel, 1 and 2 Kings, 1 and 2 Chronicles, Ezra, Nehemiah, Esther.
  Use when a query names one of these twelve books, or asks about the conquest of Canaan,
  Jericho, the judges, Deborah, Gideon, Samson, Ruth and Naomi, Samuel's call, Saul,
  David and Goliath, David's reign, Absalom, Solomon's temple, the divided kingdom,
  Elijah, Elisha, Ahab and Jezebel, Hezekiah, Josiah, the Assyrian or Babylonian exile,
  the return from exile, the rebuilding of the temple or walls, or Purim.
  Also triggers on "historical books", "former prophets", "deuteronomistic history".
---

# kanon-history

Retrieve text from the twelve historical books. This skill owns the routing decision and
the parameter contract. It does not own translation choice or the text store.

## Books Owned

Joshua, Judges, Ruth, 1 Samuel, 2 Samuel, 1 Kings, 2 Kings, 1 Chronicles, 2 Chronicles,
Ezra, Nehemiah, Esther.

Chapter ranges run from Ruth, 4 chapters, to 2 Chronicles, 36 chapters. 1 Samuel has 31,
2 Samuel 24, 1 Kings 22, 2 Kings 25, 1 Chronicles 29, Ezra 10, Nehemiah 13, Esther 10.

## Route Here When

- The query names one of the twelve books above.
- The query names a person whose narrative home is these books: Rahab, Deborah, Barak,
  Gideon, Jephthah, Samson, Delilah, Naomi, Boaz, Eli, Hannah, Samuel, Saul, Jonathan,
  David, Bathsheba, Absalom, Solomon, Rehoboam, Jeroboam, Ahab, Jezebel, Elijah, Elisha,
  Naaman, Hezekiah, Manasseh, Josiah, Nebuchadnezzar, Cyrus, Zerubbabel, Nehemiah,
  Mordecai, Haman.
- The query names a theme from these books: the fall of Jericho, the sun standing still,
  the cycle of apostasy, the ark captured by the Philistines, the anointing of David,
  the Davidic covenant, the building of the first temple, the split of the kingdom,
  the golden calves at Dan and Bethel, Mount Carmel, the chariot of fire, the Assyrian
  siege of Jerusalem, the discovery of the book of the law, the Babylonian deportation,
  the decree of Cyrus, the second temple, the wall of Jerusalem, the lot of Purim.

## Do Not Route Here

| Query looks like | Actual owner |
|---|---|
| Genesis through Deuteronomy | `kanon-law` |
| Job through Song of Solomon | `kanon-wisdom` |
| Isaiah through Malachi | `kanon-prophets` |
| Matthew through Revelation | `kanon-new-testament` |

Prophet names are the main trap. Elijah, Elisha, Nathan, Gad and the "former prophets"
label all point at narrative books, so those routes stay here. Only the books Isaiah
through Malachi go to `kanon-prophets`.

Chronicles retells Samuel and Kings. A query about a David event stays in whichever book
the user cited. Do not redirect 1 Chronicles 21 to 2 Samuel 24.

## Parameter Resolution

Emit exactly three parameters: `book`, `chapter`, `query`. The schema rejects anything
else, so do not add translation, verse, or range fields.

- `book` - normalize to an enum value from `schema.json`. Numeric names keep the numeral
  and one space: "1 Samuel", not "First Samuel" or "I Samuel". Repair "1 Sam", "2 Kgs",
  "1 Chron", "Neh", "Est".
- `chapter` - emit a bare integer. Convert spelled-out numbers. If the user names a
  person and no chapter, ask for the chapter rather than guessing, because these books
  mention most of these figures many times.
- `query` - the search intent in the user's own topic words with book and chapter
  removed. Keep the narrative wording. Do not answer in the `query` field.

## Output Contract

Return the retrieved passage alongside the normalized reference. If a chapter is out of
range, say so and offer the nearest valid chapter. Do not silently substitute.
