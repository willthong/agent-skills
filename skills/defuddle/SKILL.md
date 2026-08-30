---
name: defuddle
description: Extract clean markdown from any web page using the Defuddle CLI, stripping navigation, ads, and clutter to save tokens. Use during research whenever the user provides a URL to read, analyze, or summarize — online documentation, articles, blog posts, or any standard web page.
---

# Defuddle

Given a URL, extract the page's main content as clean, readable markdown. Defuddle removes navigation, ads, headers, footers, and other non-essential elements, so you read the article rather than the chrome around it. Prefer it over fetching the raw page when a URL is on the table.

For URLs ending in `.md`, fetch them directly — they are already markdown.

## Setup

Install once:

```bash
npm install -g defuddle
```

## Workflow

1. Extract content and metadata:
   ```bash
   defuddle parse <url> --md
   ```
2. Done when the markdown carries the page's substance and the clutter is gone. If the extraction comes back thin, empty, or the site blocks the default user agent, fall back: fetch the raw page, or retry with `--user-agent "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15"`.
3. Save a copy when the research wants one:
   ```bash
   defuddle parse <url> --md -o content.md
   ```

## Metadata

Extract a single property:

```bash
defuddle parse <url> -p title
defuddle parse <url> -p description
defuddle parse <url> -p domain
```

## Output formats

| Flag | Format |
|------|--------|
| `--md` | Markdown (default choice) |
| `--json` | JSON with both HTML and markdown |
| `--frontmatter` | Markdown with YAML frontmatter prepended |
| (none) | HTML |
| `-p <name>` | Specific metadata property |
