# Obsidian MCP Integration

This directory documents the Obsidian side of the tested integration.

## Flow

```text
Claude Desktop / MCP client
        ↓
Obsidian MCP layer
        ↓
Node.js
        ↓
Obsidian Local REST API
        ↓
Obsidian vault
```

## Tested endpoint

The local integration used HTTPS on loopback:

```text
https://127.0.0.1:27124
```

An Obsidian API key was configured locally and is intentionally not included in this repository.

## Verified operations

- Vault discovery
- Specific note read
- Vault content search
- JSON read
- Partial read of a large note

## Known limitation

A full plain-Markdown read of a large Research Card returned HTTP 500 / `errorCode: 50000`.

The JSON representation and partial content retrieval succeeded.

## Write access

Create, edit, delete, move, and rename operations were not tested.

## Configuration

Use local environment variables or ignored configuration for credentials. See `manifest.example.json` for a non-secret placeholder.
