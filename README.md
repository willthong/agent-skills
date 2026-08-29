# agent-skills

Skills for the [pi coding agent](https://pi.dev), distributed as a git package.

## Skills

| Skill | Description |
|---|---|
| `diagnose` | Disciplined diagnosis loop for hard bugs and performance regressions |
| `graphify` | Any input → knowledge graph → clustered communities → HTML + JSON + GRAPH_REPORT.md |
| `grill-with-docs` | Stress-test a plan against the project's domain language and documented decisions |
| `improve-codebase-architecture` | Find deepening opportunities informed by CONTEXT.md and docs/adr/ |
| `tdd` | Test-driven development with red-green-refactor loop |
| `to-prd` | Turn the current conversation context into a PRD |

## Install

Add the package to Pi settings (`~/.pi/agent/settings.json` for global, `.pi/settings.json` for a project):

```json
{
  "packages": ["https://github.com/willthong/agent-skills@v1"]
}
```

- Pin a tag or commit ref; pinned refs are never moved by `pi update`.
- Bump deliberately: `pi install git:github.com/willthong/agent-skills@new-ref`
- Pi clones the package to `~/.pi/agent/git/github.com/willthong/agent-skills`.
- For container setups, `entrypoint.sh` runs `pi update --extensions` on start so the clone refreshes on every `docker compose up -d` (see [pi-contained](https://github.com/willthong/pi-contained)).

> **Security:** skills can instruct the model to run arbitrary commands. Review the contents of this repository before installing it.
