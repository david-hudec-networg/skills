---
name: extract-insights
description: Mines a registered source in a UBML business-modeling workspace for candidate insights — actors, pains, opportunities, processes, constraints — and records them as UBML hypotheses (insights pending confirmation), each traceable to the exact source register entry it came from. Second step of the capture → extract-insights → promote-to-model → validate-model pipeline. Use when extracting findings from a registered transcript, interview, or document, or when asked what the discovery material actually says.
---

# Extract insights

**Contract:** one registered source in, candidate insights out — recorded as
hypotheses, because an insight is a hypothesis until a human confirms it. Each
hypothesis is a single claim in the speaker's own vocabulary, citing the source
register entry (SRC-###) it came from. No source, no hypothesis. Duplicates and
contradictions are linked, never merged or dropped. Model files stay
untouched — promotion is a separate, human-gated step.

## Ask the CLI first

```
npx --yes ubml --help                  # the command surface — the authority
npx --yes ubml show                    # what the workspace already contains
npx --yes ubml schema hypotheses       # the hypotheses document shape
npx --yes ubml add hypotheses <name>   # scaffold a NEW hypotheses document
npx --yes ubml nextid HT               # allocate tree IDs; nextid HY for hypotheses
```

Hypotheses documents hold hypothesis trees (`hypothesisTrees`, HT###) for
structured problem framing — the honest home for unconfirmed findings. `add`
scaffolds a new document only; extending an existing one is a manual edit —
see [../capture/references/cli-gaps.md](../capture/references/cli-gaps.md).

## Sequence

1. Resolve the source's register entry (SRC-###). Not registered yet? Route
   back to `capture` — extraction never invents evidence.
2. Frame one hypothesis tree per problem area the source speaks to, then walk
   the source once and emit one hypothesis per distinct claim: the claim in the
   speaker's vocabulary (consultant vocabulary beats standards vocabulary), a
   confidence honestly assigned, the SRC-### register entry it cites,
   attribution and date when known, and a short verbatim quote as context.
3. Reference existing model elements the claim is about by their typed IDs —
   check what exists with `ubml show`. If the element does not exist yet, omit
   the reference — `promote-to-model` creates elements, not this skill.
4. Link, don't merge: the same claim from a second source becomes a second
   hypothesis related to the first (convergence raises confidence, silent
   de-duplication destroys it); contradictory claims are related and both left
   unconfirmed — never pick a winner here.
5. Close the loop: every claim in the source ends up either a hypothesis or an
   explicit "ignored — not actionable" note back to the user.

## Invariants

- Every hypothesis cites a resolvable, pre-registered source entry.
- Confidence reflects evidence: direct observation high, named-source claim
  medium, hearsay low. An unattributed claim is flagged for user review, not
  silently trusted.
- A claim that sounds like a model statement ("there is a scheduling system")
  still lands as a hypothesis — promotion decides what becomes an element.
- Tree and hypothesis IDs (HT###, HY###) come from `ubml nextid <prefix>`,
  which scans the workspace — never continue a numbering by eye and never
  invent one.
