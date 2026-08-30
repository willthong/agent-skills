# Social media & communities

Twitter/X, Reddit, Facebook, Instagram.

## Twitter/X (twitter-cli)

### Auth prerequisites

Cookies saved by `agent-reach configure twitter-cookies "..."` are only used
by `agent-reach doctor` to check whether explicit credentials are present.
`doctor` does not run the upstream `twitter status`, nor does it set up the
current shell. Before running any `twitter` command below, explicitly provide
these in the same shell / child-process environment:

```bash
export TWITTER_AUTH_TOKEN="..."
export TWITTER_CT0="..."
```

### Stable commands

```bash
# Home timeline (most stable)
twitter feed -n 20

# Read a single tweet (with replies)
twitter tweet URL_OR_ID

# Read long-form posts / X Articles
twitter article URL_OR_ID

# User timeline
twitter user-posts @username -n 20

# User profile
twitter user @username
```

### Commands that may be unstable

```bash
# Search tweets (Twitter frequently changes GraphQL endpoints; may 404)
twitter search "query" -n 10

# Likes (since 2024 you can only see your own; platform limitation)
twitter likes
```

### Retry chain when search fails (run in order, stop on success)

1. Retry once directly (transient failures are common): `twitter search "query" -n 10`
2. Upgrade then retry: `pipx upgrade twitter-cli && twitter search "query" -n 10`
3. Switch to the OpenCLI fallback (desktop, reuses the browser login session): `opencli twitter search "query" -f yaml`
4. If none work, route around with stable commands like `twitter feed` / `twitter user-posts @somebody`

### Write commands (only with explicit user approval; the skill is fetch-first)

```bash
# Post a tweet (280 chars; attach up to 4 images with -i)
twitter post "TEXT" --yaml

# Unfollow / follow a user
twitter unfollow @handle
twitter follow @handle

# Delete a tweet (interactively asks "Are you sure? [y/N]" — pipe `y` to confirm)
echo y | twitter delete TWEET_ID --yaml
```

> **Success response pitfall**: a successful `twitter post`/`delete` returns
> `data: {success: true, action: post, id: ..., url: ...}` — there is **no
> `text` key**, so don't detect success by looking for tweet text. Check
> `ok: true` + `data.success == true` (or the returned `id`), then verify with
> `twitter user-posts @me` if needed.
>
> **Daily limit**: Twitter returns HTTP 0 / code 344 ("You have reached your
> daily limit for sending Tweets and messages") when the account hit its daily
> post cap — often after bulk-unfollow activity from a VPS IP. It resets
> within ~24h; until then post manually from the browser.


> **Install**: `pipx install twitter-cli` (make sure it's v0.8.5+)
>
> **Auth**: only manual Cookie-Editor export, then explicitly set the env vars
> `TWITTER_AUTH_TOKEN` + `TWITTER_CT0`; don't rely on automatic browser reading.
>
> **IP risk**: don't call frequently from VPS/datacenter IPs, especially
> followers/following — account-ban risk. Use a residential proxy or a local
> environment.
>
> **OpenCLI fallback**: with OpenCLI installed on desktop, the full
> `opencli twitter search/article/user-posts -f yaml` set works (browser login
> session, no cookie env vars needed).
>
> **Output format**: prefer `--yaml` or `--json` for structured output — more
> agent-friendly.

## Reddit (multi-backend, login required)

**Reddit has no zero-config path**: the anonymous `.json` endpoint is blocked
(403) and since 2025-11 the official API is basically never approved for new
manual applications. Both backends rely on a login session — first run
`agent-reach doctor --json` to see reddit's `active_backend`. Restricted
networks need a proxy.

### Backend A: OpenCLI (desktop preferred, reuses browser login)

```bash
# Search posts
opencli reddit search "query" -f yaml

# Read full post + comments
opencli reddit read POST_ID -f yaml

# Browse a subreddit / hot / popular
opencli reddit subreddit LocalLLaMA -f yaml
opencli reddit hot -f yaml
opencli reddit popular -f yaml

# Subreddit meta info (subscriber count, description)
opencli reddit subreddit-info LocalLLaMA -f yaml
```

> Requires Chrome open and logged into reddit.com in the browser.

### Backend B: rdt-cli (legacy/server fallback, upstream unmaintained since 2026-03)

```bash
rdt search "query" --limit 10   # search posts
rdt read POST_ID                # read full post + comments
rdt sub python --limit 20       # browse a subreddit
rdt popular --limit 10          # browse popular
rdt all --limit 10              # browse /r/all
```

> **Install**: `pipx install 'git+https://github.com/public-clis/rdt-cli.git'`
> (PyPI is behind; install v0.4.2+ from GitHub). Run `rdt login` first to
> search and read (on servers without a browser, write cookies manually per
> doctor's hints).
> Prefer `--yaml` output — more agent-friendly.

### Advanced option: official API + PRAW (only for users with existing credentials)

Users who registered a Reddit script app before 2025-11 (holding a
client_id/client_secret) can use PRAW against the official API (free 100 QPM).
New applications require manual approval and individual projects are basically
never approved — **don't recommend this path to new users**.

## Facebook (OpenCLI, login required)

Facebook goes through OpenCLI, reusing the user's facebook.com login session in
Chrome. First run `agent-reach doctor --json` to see facebook's
`active_backend` — it should be `OpenCLI`. Don't recommend Jina/Exa/Graph API
as the default path.

```bash
# Search users / pages / posts
opencli facebook search "query" -f yaml

# User or page info
opencli facebook profile zuck -f yaml

# Current account's News Feed
opencli facebook feed --limit 10 -f yaml

# Group list / recent activity visible to the current account
opencli facebook groups --limit 20 -f yaml
```

> Requires Chrome open with the OpenCLI extension installed and logged into
> facebook.com. Facebook Groups currently only promises to read the group
> list / recent activity visible to the current account — no arbitrary group
> post/comment APIs.

## Instagram (OpenCLI, login required)

Instagram goes through OpenCLI, reusing the user's instagram.com login session
in Chrome. First run `agent-reach doctor --json` to see instagram's
`active_backend` — it should be `OpenCLI`. Don't fall back to instaloader by
default; it has historically been unstable with cookies/401/429.

```bash
# Search users (NOT site-wide post keyword search)
opencli instagram search "query" -f yaml

# User profile
opencli instagram profile nasa -f yaml

# User's recent posts
opencli instagram user nasa --limit 12 -f yaml

# Explore / Discover
opencli instagram explore --limit 20 -f yaml

# Current account's saved posts
opencli instagram saved --limit 20 -f yaml
```

> Requires Chrome open with the OpenCLI extension installed and logged into
> instagram.com. `instagram search` is user search; to read posts you must
> first identify a username, then use `instagram user USERNAME`. On
> 429 / login-required errors, have the user re-login in Chrome and lower the
> request rate.
