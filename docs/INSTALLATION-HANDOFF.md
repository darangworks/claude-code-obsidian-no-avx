# Claude Code — Native Node.js / No-AVX Installation Handoff

## Purpose

This document records a tested workaround for running Claude Code on legacy Windows hardware where the native Bun-compiled executable cannot run because the CPU lacks AVX/AVX2.

## Tested environment

- Windows 10 Pro
- Intel Core 2 Duo E8400
- No AVX/AVX2
- Node.js 24.19.0
- npm 11.x
- Git

The machine does not provide the hardware virtualization support required by approaches based on WSL2, Hyper-V, or virtual machines.

## Workaround

The tested path was:

1. Obtain the Windows x64 Claude Code package used for the test.
2. Convert the Bun-compiled executable with `cc2js`.
3. Generate a Node.js-oriented bundle.
4. Run the generated `cli.js` directly with Node.js.
5. Verify Claude Code CLI commands and MCP stdio independently.
6. Connect the MCP layer to Obsidian's local REST interface.

### Tested versions

- `cc2js`: 1.2.3
- Claude Code: 2.1.270
- Node.js: 24.19.0

## Result

The conversion produced a Node-oriented bundle containing `cli.js`, `bun-shim.cjs`, native dependencies, and generated modules.

Basic CLI verification succeeded:

```text
node .\\cli.js --help
node .\\cli.js mcp --help
node .\\cli.js doctor
```

The doctor check reported no installation issues and showed Node.js invoking the generated `cli.js`.

## MCP stdio

A local test MCP server was connected through Claude Code's stdio transport.

Observed result:

```text
Status: Connected
Type: stdio
Command: node
```

Therefore:

```text
Pure Node Claude Code
        ↓
MCP stdio
        ↓
Node MCP server
        ↓
CONNECTED
```

## Important limitation

The conversion emitted a warning that the built-in plugin hooks helper was not recognised.

Therefore full compatibility of Claude Code's built-in plugin system is **not verified**.

Other capabilities that remain unverified include:

- Claude Code authentication/session
- interactive long-running sessions
- audio capture
- long-session stability
- automatic update compatibility
- complete built-in plugin behavior

## Obsidian integration

The tested Obsidian path was:

`obsidian-mcp-rest` 0.1.1 was pinned to upstream commit `b59fc7f`; its REST client accepts a configurable full base URL, including HTTPS. No TLS proxy or source patch is claimed for the tested setup.

```text
Claude Desktop
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
Obsidian Vault
```

Verified:

- vault discovery
- specific note read
- content search
- JSON note read
- partial read of a large note

A full plain-Markdown read of a large Research Card returned HTTP 500 / `errorCode: 50000`. The cause was not established, and this failure should not be generalized to the entire MCP or Node.js path.

Write operations were not tested.

## Reproduction guide

For the command-level reproduction steps, see [docs/REPRODUCTION.md](REPRODUCTION.md).

## Reproducibility principle

Keep these tests separate:

1. Node.js CLI launch
2. CLI diagnostics
3. MCP stdio transport
4. authentication/session
5. Obsidian REST connectivity
6. vault discovery
7. note read
8. content search
9. large-document behavior
10. write operations

Do not mark an untested capability as working merely because another layer works.

## Security

Credentials and private data must remain local. Never publish:

- Obsidian API keys
- Claude/API credentials
- credential caches
- private vault content
- local `C:\Users\\<username>` paths
- `.env` files

This repository documents the technical method and observed test results. It does not redistribute Claude Code binaries or proprietary Anthropic software.
