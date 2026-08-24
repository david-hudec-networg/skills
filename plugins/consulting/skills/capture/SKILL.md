---
name: capture
description: Stores raw consulting inputs — interview notes, transcripts, meeting recordings, documents — verbatim in a UBML business-modeling workspace under a sources/ folder with a register entry, evidence-backed before anything interprets them. First step of the capture → extract-insights → promote-to-model → validate-model pipeline. Use when starting a UBML workspace, registering new discovery material, or asked to model a business from raw notes.
---

# Capture

**Contract:** raw material enters the workspace verbatim-faithful — stored as a
file and registered as a source before anything cites it. No interpretation
happens here; reading meaning into the material is `extract-insights`' job.
Source IDs are register-allocated by convention; every *model* ID stays
CLI-allocated. A claim with no source does not enter the workspace.

## Ask the CLI first

```
npx --yes ubml --help            # the command surface — the authority
npx --yes ubml help quickstart   # end-to-end workspace orientation
npx --yes ubml show              # what the workspace already contains
```

`npx --yes ubml` runs the published CLI with no install. Never write a UBML file
from memory of the schema — ask the CLI, and if it rejects an edit, the edit is
wrong, not the validator.

## The sources/ folder — a workspace convention, not a CLI feature

The CLI has no document type for raw source material, so raw inputs live in a
`sources/` folder at the workspace root, indexed by a plain register file.
Register format, the SRC-numbering rule (continue from the highest entry in the
register), and the upstream issues that make this section deletable:
[references/cli-gaps.md](references/cli-gaps.md).

## Sequence

1. **Frame the workspace.** Exactly one workspace document per directory —
   scaffold a new workspace with `ubml init <name>`, never by hand. If two
   workspace files exist, stop and ask which is canonical.
2. **Store the raw material.** Each input (interview, meeting, document, email,
   observation) becomes a file under `sources/` — pasted text is saved to a
   file too, so the evidence itself lives in the repository.
3. **Register it.** Add a register entry — SRC-### ID, type, date,
   participants, pointer to the stored file — per the convention in
   [references/cli-gaps.md](references/cli-gaps.md).
4. **Keep it verbatim.** Do not summarize, clean up, or reorder the raw
   material — fidelity here is what makes later hypotheses auditable.
5. **Hand off down the pipeline.** Interpretation is `extract-insights` (once
   per source), modeling is `promote-to-model`, and nothing is committed
   without `validate-model`.

## Invariants

- Sources exist before hypotheses; hypotheses exist before model elements.
  Never shortcut a claim straight into the model.
- Source IDs (SRC-###) are the register's own counter — a documented
  convention. Every model ID (AC, EN, PR, HY, …) comes from
  `ubml nextid <prefix>`, which scans the workspace. Never invent either kind.
- Model the business twin only — the client's actors, entities, processes, and
  metrics. Delivery-side content (the consultancy's own sprints, contracts,
  staffing) stays out of the workspace.
- Exports (BPMN, diagrams) are lossy reads of the model: never commit them,
  never round-trip edits through them.
