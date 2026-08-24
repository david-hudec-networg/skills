# consulting

Business analysis and process modeling with [UBML](https://github.com/TALXIS/ubml),
TALXIS's open YAML-based business-modeling language. Four skills forming one
evidence-first pipeline: raw stakeholder material is stored verbatim under a
documented workspace convention (a `sources/` folder with a register), candidate
insights become UBML hypotheses — insights pending confirmation — confirmed
hypotheses are promoted into the operational model, and nothing commits without
validation. Each skill steers the agent to the
[`ubml` CLI](https://github.com/TALXIS/ubml) (`npx --yes ubml`) for the *how*;
the one thing the CLI can't do yet — register raw sources — is a documented
convention shipped with the upstream issues that make it deletable.

| Skill | Use when… |
|---|---|
| `capture` | storing interview notes, transcripts, or documents verbatim under `sources/` with a register entry |
| `extract-insights` | mining a registered source into hypothesis trees — candidate insights pending confirmation — with traceability |
| `promote-to-model` | promoting confirmed hypotheses into the canonical model — actors, entities, processes, metrics |
| `validate-model` | schema and cross-reference checking a workspace; gating every commit on zero errors |

## Install

```
/plugin marketplace add TALXIS/skills
/plugin install consulting@talxis
```

## Try it

Point the agent at a directory of discovery material and say "model this business
in UBML" — `capture` stores and registers the material, and the pipeline takes it
from there through hypotheses, promotion, and validation.
