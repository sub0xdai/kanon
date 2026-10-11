# kanon

![A white cross glowing on a dark background](./jesus.png)

Five skills that route biblical text queries to one of five divisions of Scripture.
The name is Greek, a rule, and so the measure by which the books are numbered and ordered.

| Division | Directory | Books | Number |
|---|---|---|---|
| Law | `kanon-law` | Genesis - Deuteronomy | 5 |
| History | `kanon-history` | Joshua - Esther | 12 |
| Wisdom | `kanon-wisdom` | Job - Song of Solomon | 5 |
| Prophets | `kanon-prophets` | Isaiah - Malachi | 17 |
| New Testament | `kanon-new-testament` | Matthew - Revelation | 27 |

The sixty-six books divide cleanly across the five, so the book alone decides the route.
The tool identifier is the directory name with underscores for hyphens, thus `kanon_law`.

## The Two Files

`SKILL.md` is for the agent: which persons, events, and themes belong to that division,
which near resemblances belong to another, and how the three parameters are resolved.
`schema.json` is for the machinery: the same description and the closed list of books, so
a wrongly offered book is refused at the threshold.

## Use

The schema is the common tool shape, being name, description, and parameters, and its
parameters are valid JSON Schema draft 2020-12.
It drops unchanged into an OpenAI-style tools array, an MCP listing, or a registry of
one's own.
Register all five, and add no sixth to gather up what the others have let fall.
A translation belongs to the store the passage is drawn from, not to the route, so add one
there.

## A Word of Caution

This instrument finds a passage.
It does not interpret it.
The Fathers read with fasting and prayer within the Tradition, and to them it was granted
to understand; no such gift is given to a machine.

## The Waiver

JSON strings cannot wrap and JSON has no comment syntax, so the long description and
book-list lines in each `schema.json` cannot fit one hundred columns without escaping
newlines into the value or cutting the detail the choosing depends on.
TB-09 is a rule for the width of code, and those lines are data.
The five frontmatter descriptions are folded YAML, which parses to the same strings, so
this is the only waiver.

```
@waiver TB-09: JSON string literals cannot wrap and JSON has no comment syntax
```
