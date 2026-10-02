# Obsidian MCP Installation Notes

This document describes the tested Obsidian-side setup boundary.

## Included

This repository includes documentation and redacted configuration examples. It does not include the tested MCPB package, Obsidian API key, private vault, or proprietary third-party binaries.

## Tested software

- obsidian-mcp-rest 0.1.1
- MCPB CLI 2.1.2
- Node.js 24.19.0
- Obsidian Local REST API configured locally

## Endpoint used in the test

```text
https://127.0.0.1:27124
```

This was the encrypted HTTPS endpoint configured in the local Obsidian REST API setup used during the successful read tests. Port `27123` is an alternative non-encrypted HTTP endpoint and was not the endpoint recorded for the successful test.

## Setup boundary

```text
Claude Desktop / MCP client
        ↓
MCPB
        ↓
Node.js
        ↓
obsidian-mcp-rest 0.1.1
        ↓
Obsidian Local REST API
```

The successful Obsidian operations were performed through Claude Desktop / MCP client. They were not performed through the converted Claude Code CLI.

## MCPB

The MCPB package used for the test is not stored in this repository. The public repository therefore documents the tested package/version boundary but does not claim byte-for-byte independent reproduction of that local MCPB artifact.

## Verification

Tested:

- vault discovery
- note read
- vault search
- JSON read
- partial large-note read

Not tested:

- create
- edit
- delete
- move
- rename

Known failure:

- full large-note Markdown read → HTTP 500 / `errorCode: 50000`

The cause of the 500 response was not established.
