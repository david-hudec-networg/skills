---
name: validate-model
description: Validates a UBML business-modeling workspace — schema and cross-reference checks through the ubml CLI — and gates every commit on zero errors. Final step of the capture → extract-insights → promote-to-model → validate-model pipeline. Use when checking a UBML business-modeling workspace before commit, after any edit to UBML files, or when ubml validation errors need diagnosing.
---

# Validate model

**Contract:** a workspace that does not validate is never committed. Zero errors
is the only pass; warnings are budgeted, not ignored. Validation never fixes by
inventing data — a failure routes back to the step that owns it: `capture` for
evidence gaps, `extract-insights` for unsourced hypotheses, `promote-to-model`
for dangling references.

## Ask the CLI first

```
npx --yes ubml --help            # the command surface — the authority
npx --yes ubml help validate     # what the validator checks, how to read output
npx --yes ubml validate .        # validate the whole workspace root
```

The validator sees UBML documents only. The `sources/` folder and its register
are a workspace convention it cannot check
([../capture/references/cli-gaps.md](../capture/references/cli-gaps.md)) —
verify by hand that every cited SRC-### entry exists and points to a real file.

## Sequence

1. **Baseline before editing.** Run the validator at the workspace root and
   record the warning count — judgement after the edit is relative to this
   baseline, not to zero.
2. **Validate after every edit.** Errors — schema violations, dangling
   references, duplicate IDs — are fixed by correcting the YAML, never by
   weakening the check. If the validator rejects an edit, the edit is wrong.
3. **Budget the warnings.** New IDs not yet referenced elsewhere produce
   transient unreferenced-ID warnings — roughly one per new ID is expected.
   Growth beyond that, or any new warning kind, is a regression to fix before
   moving on.
4. **Commit only on zero errors.** Stage the UBML files, the `sources/` files
   they cite, and the register — nothing else, and never derived exports. The
   message summarizes what was added and which source it came from.

## Invariants

- Zero errors before commit — no exceptions, no "fix it in the next commit".
- No silent fixes: never delete an element or a reference just to make
  validation pass; surface the conflict to the user instead.
- Unreferenced-element warnings alone do not block — early-stage models
  formalize progressively — but each must be intentional, not accidental.
- The validator is the CLI's. Do not re-implement its checks in ad-hoc scripts
  or reason about schema conformance from memory.
