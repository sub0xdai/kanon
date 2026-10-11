# kanon

The word is Greek, and in the usage of the Church it means a rule or a measure, and so the
measure by which the books of Holy Scripture are numbered and set in order.
To arrange the books in their divisions is not to devise a scheme of our own, but to
follow an ordering long since received among the faithful.
The five skills take their names from the Latin titles by which the Western Church has
known these same divisions.
In this way the instrument is made to serve the text, and not the text the instrument.

## The Five Divisions

The books of Scripture fall into five divisions, and among these five the skills divide
the whole of them.
No book is claimed by two skills, and no book is left unclaimed.
The sixty-six are accounted for entirely, and it is the book that is named which
determines the way.
A query can therefore never have two lawful destinations.

| Division | Directory | Books | Number |
|---|---|---|---|
| Law | `kanon-lex` | Genesis - Deuteronomy | 5 |
| History | `kanon-historia` | Joshua - Esther | 12 |
| Wisdom | `kanon-sapientia` | Job - Song of Solomon | 5 |
| Prophets | `kanon-prophetae` | Isaiah - Malachi | 17 |
| New Testament | `kanon-novum-testamentum` | Matthew - Revelation | 27 |

The identifier by which the machinery names the tool is the same name with underscores
in place of the hyphens, thus kanon_lex where the directory reads kanon-lex.

The Latin titles are these: lex is law, historia is history, sapientia is wisdom, which
the Vulgate calls the libri sapientiales, prophetae is prophets, and novum testamentum is
the New Testament under the title Jerome himself gave it.

## The Two Files

In each directory are two files, and to two different readers they speak.
The first is SKILL.md, and it is written for the agent.
It names the persons, the events, and the themes that mark a query as belonging to that
division, and it names also the near resemblances that belong to another, and it shows
how the three parameters are to be resolved when a request is loosely worded.
The second is schema.json, and it is written for the machinery that calls the skill.
It carries the same description and the closed list of books, so that a book wrongly
offered is refused at the threshold, rather than carried quietly to the wrong place.

## Bringing It Into Use

The schema follows the common shape of a tool, being name, description, and parameters.
Its parameters are valid JSON Schema draft 2020-12, and so it may be received without
alteration by an OpenAI-style tools array, by an MCP listing, or by a registry of one's
own making.
Register all five.
Let the choice be made from the description and from the book, and do not add a sixth to
gather up whatever the others have let fall, for such a one would decide nothing and
would blur the distinctions it has cost us some pains to draw.

## A Word of Caution

This instrument finds a passage.
It does not interpret it.
The Holy Fathers read the Scriptures with fasting and with prayer, and within the
Tradition of the Church, and to them it was granted to understand what they read.
No such gift has been given to a machine, and the finding of a verse is not the
understanding of it.
Let no one mistake the swiftness of the retrieval for the depth of the meaning.
The tool is a servant of the reader, and never a teacher of the faith.

## A Departure From the House Rules

One exception must be recorded plainly.
JSON strings cannot be wrapped across lines, and JSON has no comment syntax.
The long description and book-list lines in each schema.json therefore cannot be brought
within one hundred columns without escaping newlines into the value itself, or without
cutting away the very detail on which the choosing depends.
TB-09 is a rule for the width of code, and these lines are data.

```
@waiver TB-09: JSON string literals cannot wrap and JSON has no comment syntax
```

Every other finding of that rule has been corrected rather than excused, including the
five frontmatter descriptions, which are folded in the manner of YAML and parse to
strings identical to those they replaced.

## Adding a Translation

The skills know nothing of translations, and this is by design.
A translation is a property of the store from which the passage is drawn, and not of the
path by which it is found.
The skills remain the same whether the text be that of the Vulgate, the Peshitta, the
Authorized Version, or another.
To add one, add it to the store.
