---
name: promote-to-model
description: In a UBML business-modeling workspace, promotes human-confirmed hypotheses into the canonical operational model — actors, entities, processes, metrics — business entities in the model, not Dataverse tables (the implement plugin's data-model skill owns those). Third step of the capture → extract-insights → promote-to-model → validate-model pipeline. Use when hypotheses are confirmed and the UBML business model should be created or extended, or when asked to add actors, entities, or processes to a UBML workspace.
---

# Promote to model

**Contract:** the model changes only here, and only from hypotheses a human has
confirmed — nothing is promoted without one. Every promoted element traces to
at least one cited hypothesis; every field written is evidenced. No
placeholders, no padding "for completeness", no guessing.

## Ask the CLI first

```
npx --yes ubml --help              # the command surface — the authority
npx --yes ubml show                # read the current model before changing it
npx --yes ubml schema <type>       # element shapes per document type
npx --yes ubml nextid <prefix>     # allocate every new ID (AC, EN, PR, ST, …)
```

`show` lists what actually exists in the workspace — `ids` does not; it prints
a static ID-pattern cheat sheet. `add <type> <name>` scaffolds a NEW document
only; extending an existing one is a manual edit — see
[../capture/references/cli-gaps.md](../capture/references/cli-gaps.md).

## Sequence

1. Gather the candidate hypotheses (`ubml show`) and confirm the gate with the
   user: which are confirmed — corroborated or human-validated — and cleared
   for promotion. Low-confidence or contested hypotheses stay hypotheses.
2. Decide the element type from the evidence: people, roles, teams, and systems
   become actors; records and documents become entities; activities become
   processes with steps; measurements become metrics. When a name is ambiguous,
   the source's verb decides ("maintains the checklist" → entity, "runs the
   checklist" → process); still ambiguous → ask, don't guess.
3. Allocate every new ID with `ubml nextid <prefix>` — it scans the workspace,
   so it stays consistent with the numbering already in the files.
4. Refine before creating: a hypothesis about an existing element updates it
   with evidenced fields rather than duplicating it under a new name. Two names
   for one actor ("dispatcher" vs "dispatch coordinator" at a field-service
   company) become one element with the alias recorded in its description.
5. Wire relationships by typed ID references, never by name strings. If the
   ordering between process steps is not stated in any hypothesis, omit the
   link rather than inventing a flow.
6. Record provenance: note the backing hypothesis IDs (HY###) and their source
   register entries (SRC-###) alongside each promoted element.
7. Finish with `validate-model` — promotion is not done until the workspace
   validates.

## Invariants

- No element without a confirmed hypothesis behind it; no field without
  evidence in a registered source.
- No stored aggregations ("total duration", "average cost") — derive at read
  time. No version or audit fields — git is the version control.
- One parent per element; no dual hierarchies.
