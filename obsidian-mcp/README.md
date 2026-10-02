# Obsidian MCP Integration

This directory documents the Obsidian side of the tested integration.

## Tested architecture

```text
Claude Desktop / MCP client
        ↓
MCPB
        ↓
Node.js
        ↓
obsidian-mcp-rest 0.1.1
        ↓
HTTPS 127.0.0.1:27124
        ↓
Obsidian Local REST API
        ↓
Obsidian vault
```

## Tested endpoint and bridge provenance

The successful local test used:

```text
https://127.0.0.1:27124
```

The tested `obsidian-mcp-rest` release was **0.1.1**, corresponding to upstream commit `b59fc7f`. Its REST client accepts a configurable full base URL, including HTTPS. Therefore the HTTPS scheme is supported by the bridge itself.

Port `27123` is the alternative non-encrypted HTTP endpoint and was not the endpoint recorded for the successful test. Do not infer the endpoint solely from an upstream default; reproduce the local configuration used by the test.

## Tested client boundary

The Obsidian operations were tested through **Claude Desktop / MCP client**, not through the converted Claude Code CLI.

Therefore this repository establishes a separate Obsidian read path, not a complete Claude Code → Obsidian workflow.

## Tested software versions

- `obsidian-mcp-rest`: 0.1.1
- Upstream source commit: `b59fc7f`
- Node.js: 24.19.0
- MCPB CLI: 2.1.2
- Local REST API: configured locally; exact plugin release is not recorded in the public repository

The MCPB package itself is **not included** in this repository.

## Verified operations

- Vault discovery
- Specific note read
- Vault content search
- JSON read
- Partial read of a large note

## Known limitation

A full plain-Markdown read of a large Research Card returned HTTP 500 / `errorCode: 50000`.

The JSON representation and partial content retrieval succeeded.

The cause was not established. This should not be generalized into a failure of the complete MCP/Node/REST stack.

## Write access

Create, edit, delete, move, and rename operations were not tested.

## Credentials

An Obsidian API key was configured locally and is intentionally excluded from this repository.

Use a local environment variable or ignored configuration. Never commit the real key.

See:

- [Installation](INSTALLATION.md)
- [Example configuration](config.example.json)
