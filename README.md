# agent-skills

Skills for the [pi coding agent](https://pi.dev), distributed as a git package.

## Provenance

This repo mirrors [mattpocock/skills](https://github.com/mattpocock/skills) — the engineering skills plus `teach` and `writing-for-agents` (from upstream's `productivity/` category), copied from upstream at the top level of `skills/`, replacing the previously customized local content. The rest of `productivity/`, `in-progress/`, and `misc/` are intentionally not mirrored. Last synced from upstream commit `6654f6b` (2026-08-24).

Localizations applied on top of upstream:

- **Issue tracking → GitHub**: skills assume GitHub issues (via the `gh` CLI) with the canonical triage labels; `wayfinder` inlines the GitHub wayfinding operations directly.
- **Skill loading → pi**: "call the Skill tool with X" phrasing is rewritten as "load the `X` skill" (pi loads skills by reading their `SKILL.md`). Category README harness notes referenced pi's `disable-model-invocation`.
- **Removed-skill references trimmed**: `ask-matt` (the router) routes only over shipped skills. The `grilling` interview primitive (from upstream's `productivity/` category) is included alongside the engineering skills because several of them load it.

## Community imports

On top of the upstream mirror, four community skills are imported from their own upstream repos, adapted to this repo's conventions and re-passed through `writing-for-agents`:

- **`defuddle`** (from [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills)) — extract clean markdown from a web page via the Defuddle CLI. Model-invoked: loads during research whenever a URL is to be read, analyzed, or summarized.
- **`agent-reach`** (from [thanhtantran/agent-reach-hermes](https://github.com/thanhtantran/agent-reach-hermes)) — search and read across 10+ internet platforms via the `agent-reach` CLI, with per-platform backends in `references/`. Model-invoked: loads when the user wants to find or look something up online. Plain web-page URLs prefer `defuddle`.
- **`humanizer`** (from [blader/humanizer](https://github.com/blader/humanizer)) — remove AI-writing patterns from prose without changing what it says. Model-invoked: loads whenever writing, editing, or reviewing a report.
- **`caveman`** (from [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)) — ultra-compressed terse replies that keep full technical accuracy. User-invoked: hidden from the agent and loaded only when the human types `/skill:caveman` — never on its own initiative.

## Skills

`ask-matt`, `code-review`, `codebase-design`, `diagnosing-bugs`, `domain-modeling`, `grill-with-docs`, `grilling`, `implement`, `improve-codebase-architecture`, `prototype`, `research`, `resolving-merge-conflicts`, `tdd`, `teach`, `to-spec`, `to-tickets`, `triage`, `wayfinder`, `wizard`, `writing-for-agents`

Plus the community imports: `agent-reach`, `caveman`, `defuddle`, `humanizer` (see [Community imports](#community-imports)).

See each skill's `SKILL.md` and `ask-matt` (the router) for when to use them.

## Install

Add the package to Pi settings (`~/.pi/agent/settings.json` for global, `.pi/settings.json` for a project):

```json
{
  "packages": ["https://github.com/willthong/agent-skills"]
}
```

- Without a ref, Pi tracks the default branch (`main`) and pulls the latest commit on `pi update` / `pi update --extensions`.
- To pin for reproducibility, append a tag or commit ref (e.g. `@v1`); pinned refs are reconciled but never moved by updates. Bump deliberately with `pi install git:github.com/willthong/agent-skills@new-ref`.
- Pi clones the package to `~/.pi/agent/git/github.com/willthong/agent-skills`.
- For container setups, `entrypoint.sh` runs `pi update --extensions` on start so the clone refreshes on every `docker compose up -d` (see [pi-contained](https://github.com/willthong/pi-contained)).

> **Security:** skills can instruct the model to run arbitrary commands. Review the contents of this repository before installing it.
