# Contributing

## This fork

This fork is a preview channel for TALXIS/skills: it ships changes before, or instead of, proposing them upstream.

- `master` is an exact copy of TALXIS `master`. Nothing is committed to it directly.
- Cut every branch from `master` and open its PR into this fork's `preview`, where it merges once the work is finished. A PR to TALXIS/skills is opened only when its maintainers want the change.
- A fix to work still in its PR goes on the same branch; anything after the merge gets a new branch.
- `preview` is the default branch and the one people install from. It takes upstream changes from `master` through a PR whenever TALXIS moves; where they clash with work here, upstream wins and the work is rebuilt on top.
- Releases are cut from `preview` with a `-preview.N` version.
- Commits that only make the fork installable (the `talxis-preview` marketplace name and the pointers to the ubml and txc preview releases) live on `preview` alone, never on a PR branch.
- This repository is public: no customer names, project codes, internal paths or people's names in code, commits, branch names or PR text.

## The two rules

1. **Skills are organized by universal developer intent** (tables, backend logic,
   screens, tests, deployment) — never by platform vocabulary. The consuming agent
   knows nothing about Power Platform and shouldn't need to.
2. **The CLI answers "how"; markdown never duplicates it.** Steer the agent to
   `txc component type list/explain`, `txc workspace component parameter list <type>`,
   and `txc docs get <id>`. If you're about to write prose working around a tool
   limitation, add the gap to [TOOLING-BACKLOG.md](TOOLING-BACKLOG.md) instead and
   keep the workaround in a reference file that names it.

   Scaffold templates run post-actions that can fail without an error
   (TALXIS/tools-devkit-templates#177): for each command a reference says what the
   outcome should be and how to check it (`dotnet sln list`, a generated file, an entry
   in `Solution.xml`), so the agent can verify it and repair what is missing.

## Skill standard (enforced by `scripts/validate.mjs` on every PR)

- Body skeleton: contract line → "ask the CLI first" → intent→type mapping →
  sequence → invariants → references. Target ≤ ~70 lines.
- No `--param` enumerations in SKILL.md — `parameter list` is the authority. A
  specific parameter may appear only when it encodes an invariant.
- Zero toolchain prose: no version checks, installs, or updates. The `init`
  skill points to `agentbox`, which owns them. Machine-checked.
- Frontmatter: single-line `name:` (= directory name) and `description:` (≤ 1024
  chars, contains "Use when …", platform-agnostic vocabulary with Dataverse/Power
  Platform as trigger keywords only).
- Host-agnostic: no `~/.claude`-style paths, no tokens beyond
  `${PLUGIN_ROOT}` / `${PLUGIN_DATA}`.
- Body ≤ 5000 tokens; > 4000 requires `references/`.

## References standard (machine-checked)

Every `skills/*/references/*.md` starts with:

```
> **Needed because:** <the txc gap>
> **Remove when:** <TOOLING-BACKLOG item / tools-cli change>
```

When the named fix ships, delete the file and shrink the skill.

## Manifests

`plugin.json` (Agent Plugins 1.0) and, for plugins that ship an MCP server, `mcp.json` are the source of truth;
`.claude-plugin/plugin.json` and `.mcp.json` are generated — never edit them:

```
node scripts/generate-manifests.mjs && node scripts/validate.mjs
```

`version` lives only in `plugin.json`: skill removed/renamed → major, skill added →
minor, fixes → patch. Placeholder plugins (README-only) are not installable and not
listed in the marketplace.

## Local testing

- Claude Code: `claude --plugin-dir ./plugins/implement`
- Copilot CLI: `copilot plugin install <abs-path>/plugins/implement`
  (Copilot copies at install time — reinstall to pick up edits)
