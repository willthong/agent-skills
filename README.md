# agent-skills

Skills for the [pi coding agent](https://pi.dev), distributed as a git package.

## Provenance

This repo mirrors [mattpocock/skills](https://github.com/mattpocock/skills) — **all 18 engineering skills**, copied from upstream at the top level of `skills/`, replacing the previously customized local content. The `productivity/`, `in-progress/`, and `misc/` categories are intentionally not mirrored. Last synced from upstream commit `6654f6b` (2026-08-24).

Localizations applied on top of upstream:

- **Issue tracking → GitHub**: `setup-matt-pocock-skills` proposes GitHub issues (via the `gh` CLI) as the only tracker; the GitLab and local-markdown tracker options/templates were removed. `wayfinder` defaults to GitHub issues when no tracker is configured.
- **Skill loading → pi**: "call the Skill tool with X" phrasing is rewritten as "load the `X` skill" (pi loads skills by reading their `SKILL.md`). Category README harness notes referenced pi's `disable-model-invocation`.
- **Removed-skill references trimmed**: `ask-matt` (the router) routes only over shipped skills; the `grilling` primitive (in the dropped `productivity/` category) is referenced as an interview style rather than a loadable skill.

## Skills

`ask-matt`, `code-review`, `codebase-design`, `diagnosing-bugs`, `domain-modeling`, `grill-with-docs`, `implement`, `improve-codebase-architecture`, `prototype`, `research`, `resolving-merge-conflicts`, `setup-matt-pocock-skills`, `tdd`, `to-spec`, `to-tickets`, `triage`, `wayfinder`, `wizard`

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
