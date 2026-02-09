# Kubera Skill

Read and manage your [Kubera](https://www.kubera.com) portfolio data from the command line or any AI agent.

## What it does

- **Net worth summary** — total assets, debts, allocation breakdown, top holdings
- **Asset listing** — filter by sheet (Equities, Crypto, Retirement), sort by value/name/gain
- **Search** — find holdings by name, ticker, or account
- **Full JSON export** — raw API data for custom analysis
- **Update assets** — modify values with a `--confirm` safety gate

Zero external dependencies — Python standard library only.

## Quick Start

1. Get your API key and secret from **Kubera Settings > API**
2. Export credentials:
   ```bash
   export KUBERA_API_KEY="your-api-key"
   export KUBERA_SECRET="your-api-secret"
   ```
3. Run:
   ```bash
   python3 scripts/kubera.py summary
   ```

## Commands

```bash
# Net worth + allocation + top 10 holdings
python3 scripts/kubera.py summary

# List all assets (optionally filter/sort)
python3 scripts/kubera.py assets --sheet Crypto --sort value

# Search by name or ticker
python3 scripts/kubera.py search "bitcoin"

# Full portfolio as JSON
python3 scripts/kubera.py json

# List portfolios (multi-portfolio accounts)
python3 scripts/kubera.py portfolios

# Update an asset value (requires --confirm)
python3 scripts/kubera.py update <ITEM_ID> --value 5000 --confirm
```

Add `--json` to `summary`, `assets`, `search`, or `portfolios` for machine-readable output.

## AI Agent Usage

This skill follows the [AgentSkills](https://agentskills.io) format. Drop the folder into your agent's skills directory:

- **OpenClaw**: `~/.openclaw/skills/kubera/` or `<workspace>/skills/kubera/`
- **Claude Code / Codex / Others**: Any directory your agent can access

Set `KUBERA_API_KEY` and `KUBERA_SECRET` as environment variables and the agent will be able to query your portfolio.

## API Coverage

Covers all [Kubera Data API V3](https://help.kubera.com/article/133-kubera-api) endpoints:

| Endpoint | Command |
|----------|---------|
| `GET /api/v3/data/portfolio` | `portfolios` |
| `GET /api/v3/data/portfolio/{id}` | `summary`, `assets`, `search`, `json` |
| `POST /api/v3/data/item/{id}` | `update` |

## Rate Limits

- 30 requests/minute
- 100 requests/day (Essentials) or 1,000/day (Black)

## Security

- Credentials via environment variables only — never hardcoded
- HTTPS only, HMAC-SHA256 signed requests
- Write operations require explicit `--confirm` flag
- Read-only API keys recommended for most use cases

## License

MIT
