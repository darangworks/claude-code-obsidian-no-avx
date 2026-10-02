# Reproduction Guide

This guide records the reproducible parts of the tested setup without distributing Claude Code binaries, generated proprietary output, credentials, or private Obsidian data.

## Tested environment

- Windows 10 Pro x64
- Intel Core 2 Duo E8400
- No AVX/AVX2
- Node.js 24.19.0
- npm 11.x
- Git

## Claude Code / Node.js path

### Versions

- Claude Code: 2.1.270
- cc2js: 1.2.3
- Node.js: 24.19.0

### Install cc2js

```powershell
npm install -g cc2js@1.2.3
cc2js --help
```

### Convert

Use a legitimately obtained Windows x64 `claude.exe` for the tested Claude Code release:

```powershell
cc2js "<path-to-claude.exe>" --no-link --out "<output-directory>"
```

The tested command generated a Node-oriented output directory containing `cli.js`, `bun-shim.cjs`, native dependencies, and generated modules.

Do not commit that generated directory.

### Verify

```powershell
cd "<output-directory>"
node .\cli.js --help
node .\cli.js mcp --help
node .\cli.js doctor
```

The recorded test showed the CLI starting under Node.js and `doctor` reporting no installation issues.

### MCP stdio

The recorded MCP test used a generic local Node MCP server, not the Obsidian server:

```text
Status: Connected
Type: stdio
Command: node
```

This verifies transport connectivity only.

## Obsidian path

The Obsidian integration was tested separately through Claude Desktop / MCP client.

Tested versions:

- obsidian-mcp-rest 0.1.1
- MCPB CLI 2.1.2
- Node.js 24.19.0

The MCPB package used in the local test is not included in this repository.

### Endpoint

The successful local test used:

```text
https://127.0.0.1:27124
```

This was the encrypted HTTPS endpoint configured in the local Obsidian REST API setup used for the test. Port `27123` was not the recorded endpoint for that successful test.

### Credentials

The Obsidian API key is local-only. Configure it through local environment/configuration and never commit it.

A redacted example is provided in `obsidian-mcp/config.example.json`.

### Verified read path

The following were observed through the Claude Desktop Obsidian path:

- vault discovery
- specific note read
- vault search
- JSON read
- partial read of a large note

A full plain-Markdown read of a large Research Card returned HTTP 500 / `errorCode: 50000`. The cause was not established.

Write operations were not tested.

## Reproducibility boundary

This repository makes a distinction between the specific local workflow that was documented and tested, and capabilities that remain unestablished: universal no-AVX compatibility, future Claude Code versions, arbitrary MCP servers, Claude Code authentication/session behavior, long-running stability, audio capture, automatic update compatibility, built-in plugin compatibility, and Claude Code → Obsidian end-to-end use.

For an exact third-party reproduction, the precise source artifact/checksum of the tested Claude Code executable and the exact local MCPB build inputs would still need to be recorded. They are intentionally not distributed here.
