# Jobs & Recruiting

LinkedIn.

## LinkedIn (MCP server: `mcp-server-linkedin`, via uvx)

**Setup (verified on aarch64):** MCP server runs as a local HTTP server registered in mcporter:

```bash
# install (uvx resolves Python 3.12+ automatically; no venv needed)
mcporter config add linkedin http://localhost:3000/mcp --scope home
uvx mcp-server-linkedin@latest --transport streamable-http --host 127.0.0.1 --port 3000 --path /mcp
```

**Login:** first tool call opens a browser login window (30 min default; needs a visible display, e.g. `ssh -X`). Profile persists at `~/.linkedin-mcp/profile/`. The user types credentials themselves. Alternative: `uvx mcp-server-linkedin@latest --import-from-browser` to reuse a local Chromium session, or `--login` for an explicit login run.

**Tools (actual schema — params differ from older docs):**

```bash
# Get a person's profile — param is linkedin_username (NOT linkedin_url)
mcporter call 'linkedin.get_person_profile(linkedin_username: "williamhgates")'

# Search for people
mcporter call 'linkedin.search_people(keyword: "AI engineer", limit: 10)'

# Get a company profile — param is company_name (NOT linkedin_url)
mcporter call 'linkedin.get_company_profile(company_name: "Gates Foundation")'

# Search jobs
mcporter call 'linkedin.search_jobs(keyword: "software engineer", limit: 10)'
```

> **Login required**: the LinkedIn scraper needs a valid login session.

### Pitfalls (encountered in the field)

- `doctor` shows `linkedin -> warn` **even when fully working**: Doctor does not start the local server for a connectivity probe. Verify with a real `mcporter call` instead.
- `"A previous browser on this profile did not shut down cleanly... Restart the server to recover."` → kill and restart the uvx process, then call again.
- `"Session expired. A login browser window has been opened."` / `"Stale session detected"` → saved session rejected; a new login window opens — complete sign-in, then retry the exact tool call (~30s later).
- Do not let the login window idle past the 30-min timeout; the session saves only after successful sign-in.
- LinkedIn is aggressive about automation: use a secondary account, low call frequency, residential proxy if possible.

### Fallback

If the MCP is unavailable, Jina Reader is an option but **currently blocked**: anonymous Jina access to `linkedin.com` returns 403 AbuseAlleviationError (DDoS ban), so prefer the MCP.

