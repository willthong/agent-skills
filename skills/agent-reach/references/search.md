# Search tools

Exa AI search engine.

## Exa AI Search

High-quality AI search engine, strong at technical and code search.

```bash
mcporter call 'exa.web_search_exa(query: "query", numResults: 5)'
mcporter call 'exa.get_code_context_exa(query: "code question", tokensNum: 3000)'
```

### Use cases

| Scenario | Parameters |
|-----|------|
| Web search | `web_search_exa(query: "...", numResults: 5)` |
| Code search | `get_code_context_exa(query: "...", tokensNum: 3000)` |

### Strengths

- Strong on English content and technical docs
- Supports code-context search
- High-quality results

## Compared to other search tools

| Tool | Source | Best for |
|-----|------|---------|
| Exa | agent-reach | English / technical / code search |
| Zhipu search | my-mcp-tools | Chinese-language search |
| GitHub search | [agent-reach](dev.md) | Repo / code search |
