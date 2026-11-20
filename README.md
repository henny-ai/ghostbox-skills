# GhostBox skills

[GhostBox](https://ghostbox.me) gives each site its own email alias that forwards to your real inbox.
The `ghostbox` skill teaches any AI agent to hand out an alias instead of your real address, reuse
one alias per site, and pause instead of delete.

## Install the skill

```bash
git clone https://github.com/henny-ai/ghostbox-skills
cp -r ghostbox-skills/ghostbox ~/.claude/skills/ghostbox   # Claude Code
cp -r ghostbox-skills/ghostbox ~/.codex/skills/ghostbox    # OpenAI Codex
```

Other agents that read the `SKILL.md` format take the same folder in their own skills directory.
In the Claude app, upload the `ghostbox` folder as a skill. An agent with no skill support (Cursor
rules, an `AGENTS.md`, a system prompt) can take the body of `ghostbox/SKILL.md` as instructions.

## Connect the MCP server

The skill works best with GhostBox's MCP tools. Create an API key in the GhostBox app (Settings,
API keys; Pro plan), keep it in the `DECOY_API_KEY` environment variable, and send it as a header:

- Endpoint: `https://api.decoy.henny.ai/mcp` (Streamable HTTP)
- Header: `Authorization: Bearer <your API key>`

Cursor (`~/.cursor/mcp.json`):

```json
{ "mcpServers": { "ghostbox": {
  "url": "https://api.decoy.henny.ai/mcp",
  "headers": { "Authorization": "Bearer ${env:DECOY_API_KEY}" } } } }
```

Other clients take the same URL and header; check how yours reads an environment variable in its
config.

Claude Code: `claude mcp add --transport http ghostbox https://api.decoy.henny.ai/mcp --header "Authorization: Bearer $DECOY_API_KEY"`

Connectors that require OAuth (claude.ai and ChatGPT custom connectors) can't connect yet.

The REST API and its OpenAPI document (`https://api.decoy.henny.ai/v1/openapi.json`) work from any
agent that can run `curl`.
