---
name: agent-reach
description: Search and read across 10+ internet platforms — Twitter/X, Reddit, Facebook, Instagram, LinkedIn, YouTube, GitHub, RSS, and web search — via the agent-reach CLI with per-platform backends. Use when the user wants to find or look something up online, on a specific platform or across several, or shares a link from one. For a plain web-page URL to extract as clean markdown, prefer the defuddle skill.
license: MIT
---

# Agent Reach — internet capability router

Search and read across platforms, each served by multiple backends. **When this skill is loaded, use it for these platforms — do not invent your own approach.**

## When to use

- User asks to research, search, or look anything up on the internet
- User mentions a supported platform or shares a URL from one (Twitter/X, Reddit, Facebook, Instagram, LinkedIn, YouTube, GitHub, RSS, web pages)
- Broad research spanning multiple sources — combine platforms (Exa for web search, Twitter/Reddit for discussions, Facebook/Instagram for social perspectives), collect in parallel, then synthesize

Not for: writing reports or analysis (the humanizer skill does that), posting/commenting/liking, or platforms that have their own dedicated skill (prefer that skill).

## First-use setup

This skill is a router for the `agent-reach` CLI — useless without it. Check, install, and configure once:

```bash
command -v agent-reach || pip install agent-reach   # or: pipx install agent-reach
agent-reach install --env=auto
```

- `agent-reach install --env=auto` sets up core infrastructure (gh CLI, Node.js, mcporter, Exa search, yt-dlp config) and activates the zero-config channels: Web (Jina Reader), YouTube, GitHub, RSS, Exa Search. Preview first with `--safe` (check only) or `--dry-run`.
- Login-backed channels (Twitter, Reddit, Facebook, Instagram, LinkedIn) are installed on demand: ask the user which they need, then run `agent-reach install --env=auto --channels=twitter,reddit,...`. Never install `--channels=all` without asking.
- If install wants to change system-level things (sudo, system packages) without approval, stop and tell the user. Full guide and boundaries: the upstream repo's [README](https://github.com/thanhtantran/agent-reach-hermes).

## Standing rules

1. **Health-check before acting**: for login-backed platforms (Reddit / Twitter / Facebook / Instagram), run `agent-reach doctor --json` first and pick the command group matching each platform's `active_backend`.
2. **Announce what you use**: say "using agent-reach, platform X via backend Y" before starting.
3. **On failure, follow the retry chains in references/** — never guess commands.
4. **Workspace**: never create files in the agent workspace. Use `/tmp/` for temporary output and `~/.agent-reach/` for persistent data.

## Routing table

| User intent | Reference |
|-------------|-----------|
| Web / code search | [references/search.md](references/search.md) |
| Twitter / Reddit / Facebook / Instagram | [references/social.md](references/social.md) |
| Jobs / LinkedIn | [references/career.md](references/career.md) |
| GitHub / code | [references/dev.md](references/dev.md) |
| Web pages / articles / RSS | [references/web.md](references/web.md) |
| YouTube / audio transcription | [references/video.md](references/video.md) |

## Zero-config quick commands

```bash
# Exa web search
mcporter call 'exa.web_search_exa(query: "query", numResults: 5)'

# Read any web page
curl -s "https://r.jina.ai/URL"

# GitHub search
gh search repos "query" --sort stars --limit 10

# YouTube subtitles
yt-dlp --write-sub --skip-download -o "/tmp/%(id)s" "URL"
```

## Login-backed platforms

Pick commands by the `active_backend` that `agent-reach doctor --json` reports.

Twitter boundary: cookies saved by `agent-reach configure twitter-cookies` are used only by `doctor` to check whether explicit credentials are present. `doctor` does not run `twitter status` or configure the current shell. Before calling `twitter` directly, explicitly provide `TWITTER_AUTH_TOKEN` and `TWITTER_CT0` in the child-process environment without logging their values.

```bash
# Twitter search (twitter-cli preferred; retry chain in social.md)
twitter search "query" -n 10

# Reddit (no zero-config path — OpenCLI or rdt-cli, login required)
opencli reddit search "query" -f yaml   # desktop
rdt search "query" --limit 10           # legacy/server

# Facebook / Instagram (desktop OpenCLI, browser session)
opencli facebook search "query" -f yaml
opencli facebook groups -f yaml
opencli instagram search "query" -f yaml       # user search
opencli instagram user USERNAME -f yaml        # recent posts from one user
```

## Environment check

```bash
# Channel availability + which backend serves each platform
agent-reach doctor --json
```

## Detailed references

Read the matching file when you need specifics — the commands above cover the common cases; references hold per-backend command groups, caveats, and retry chains:

- [Search](references/search.md) — Exa AI search
- [Social](references/social.md) — Twitter, Reddit, Facebook, Instagram (multi-backend/login-backed groups)
- [Career](references/career.md) — LinkedIn
- [Dev](references/dev.md) — GitHub CLI
- [Web](references/web.md) — Jina Reader, web-reader MCP, RSS
- [Video](references/video.md) — YouTube subtitles, audio transcription
