> **Needed because:** the ubml CLI has no document type or ID allocation for raw source material, and `add` scaffolds new documents only (no append).
> **Remove when:** TALXIS/ubml ships a source-register document type + nextid coverage for it, and an append/add-entry mode (issues to file on github.com/TALXIS/ubml — list below).

# CLI gaps and the conventions that cover them

Everything in this file is a workspace convention layered on top of ubml 1.3.0,
not a CLI feature. The CLI stays the authority for everything it does cover:
document types are workspace, actors, process, entities, metrics, hypotheses,
scenarios, strategy, glossary, views, links, and mining — no source or insight
types exist, and no SR or IN prefixes.

## The source register (convention, not CLI)

Raw stakeholder material lives in a `sources/` folder at the workspace root,
next to the UBML documents. One plain register file, `sources/register.md`,
indexes it — one entry per source:

```markdown
## SRC-001 — Interview, dispatch team lead
- type: interview
- date: 2026-08-24
- participants: Jana N. (dispatch team lead), David H. (consultant)
- file: ./2026-08-24-dispatch-lead-interview.md
```

The stored file holds the raw material verbatim — never summarized, cleaned up,
or reordered.

### SRC numbering

Source IDs are convention-allocated: read the register, take the highest
existing SRC-### and continue from it. Never renumber, never reuse a number,
never invent a parallel scheme. This is the one ID family the CLI cannot
allocate — every model ID (AC, EN, PR, ST, HY, HT, EV, SC, …) comes from
`ubml nextid <prefix>`, which scans the workspace. (`ubml ids` allocates
nothing: it prints a static ID-pattern cheat sheet; `ubml show` is what lists
workspace contents.)

## `add` scaffolds — it does not append

`ubml add <type> <name>` always creates a NEW document. To extend an existing
document: allocate the ID with `ubml nextid <prefix>` first, add the entry by
hand after the last one in the document's top-level map, then run
`ubml validate .`. If the validator rejects the edit, the edit is wrong.

## Issues to file on github.com/TALXIS/ubml

1. **Source-register document type + allocator coverage** — a document type for
   registering raw source material (metadata + file pointers) with a prefix
   that `nextid` covers, so evidence capture stops being a convention.
2. **`add --append` (or an add-entry mode)** — `add` only scaffolds new
   documents; adding an entry to an existing document is manual YAML editing
   today.
3. **`syntax hypothesis` bug** — in 1.3.0, `ubml syntax hypothesis` answers
   "Unknown element type" while its own error message lists `hypothesis` among
   the available element types.
