# Demandbase MCP Plugin

Use [Demandbase](https://www.demandbase.com) account intelligence from LLM clients that support MCP servers and plugin manifests.

## What It Enables

- Ask natural-language questions about your Demandbase tenant: accounts, people, opportunities, engagement, intent, and buying-group coverage
- Look up Demandbase's global B2B intelligence: firmographics, technographics, corporate hierarchy, news, and employment information
- Read-only access that respects your existing Demandbase permissions

## Supported Clients

| Client | Manifest |
|---|---|
| Claude Code | `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` |
| Cursor | `.cursor-plugin/plugin.json`, `.cursor-plugin/marketplace.json` |
| OpenAI Codex | `.codex-plugin/plugin.json` |
| Gemini CLI | `.gemini-plugin/plugin.json` |
| Any MCP client | `.mcp.json` |

All manifests point to the same `.mcp.json`. When releasing, keep `version` in sync across every `plugin.json`.

## Prerequisites

- An LLM client that supports remote MCP servers
- A Demandbase account with Demandbase MCP enabled for your organization

## MCP Server

The plugin registers Demandbase's hosted MCP server (`https://gateway.demandbase.com/mcp/servers/db-mcp`). Authentication is handled through your Demandbase sign-in via OAuth (Authorization Code + PKCE). No API keys are stored in this repo.

```json
{
  "mcpServers": {
    "demandbase": {
      "type": "http",
      "url": "https://gateway.demandbase.com/mcp/servers/db-mcp"
    }
  }
}
```

Client-specific setup guides: https://developer.demandbase.com/docs/mcp

## Official MCP Registry

This server is listed in the [official MCP Registry](https://registry.modelcontextprotocol.io) as `io.github.Demandbase/demandbase-mcp`. The listing is defined by `server.json`.

To publish a new version, create a GitHub release tagged `vX.Y.Z`. The `publish-mcp-registry` workflow stamps that version into `server.json`, validates it, authenticates through GitHub OIDC, and publishes. Each version can be published only once.

Verify the listing:

```bash
curl -s "https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.Demandbase/demandbase-mcp"
```

## Project Structure

```text
.claude-plugin/
  plugin.json
  marketplace.json
.codex-plugin/
  plugin.json
.cursor-plugin/
  plugin.json
  marketplace.json
.gemini-plugin/
  plugin.json
.github/workflows/
  publish-mcp-registry.yml
.mcp.json            # MCP server registration (all clients)
server.json          # MCP Registry metadata
CHANGELOG.md
```
