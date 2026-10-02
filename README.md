# Claude Code → Obsidian on No-AVX Windows

A documented path for running Claude Code on legacy Windows systems without AVX/AVX2 using a Node.js-compatible Claude Code build, plus a separately verified Obsidian MCP/REST integration.

## Target environment

- Windows 10 x64
- Legacy CPU without AVX/AVX2
- Node.js 24.x
- No WSL2, Hyper-V, or VM dependency

## What is verified

### Claude Code path

```text
Claude Code
    ↓
Pure Node.js CLI
    ↓
MCP / stdio
    ↓
Node MCP server
```

The Pure Node Claude Code CLI and its MCP stdio transport were verified independently.

### Obsidian path

```text
Claude Desktop / MCP client
    ↓
MCPB
    ↓
Node.js
    ↓
Obsidian MCP REST layer
    ↓
Obsidian Local REST API
    ↓
Obsidian vault
```

The Obsidian integration was verified separately through Claude Desktop. This repository does **not** claim that the complete Claude Code → Obsidian end-to-end path has been verified.

## Verified status

| Component | Status |
|---|---|
| Native Node.js Claude Code conversion | VERIFIED |
| Claude Code CLI launch | VERIFIED |
| Claude Code MCP stdio | VERIFIED |
| Obsidian MCP integration | VERIFIED |
| Obsidian REST connectivity | VERIFIED |
| Vault discovery | VERIFIED |
| Specific note read | VERIFIED |
| Vault content search | VERIFIED |
| Large-note JSON read | VERIFIED |
| Large-note Markdown read | FAILED — HTTP 500 |
| Write operations | NOT TESTED |
| Claude Code authentication/session | NOT VERIFIED |
| Full built-in plugin compatibility | NOT VERIFIED |
| Claude Code → Obsidian end-to-end workflow | NOT VERIFIED |

## Documentation

- [Installation handoff](docs/INSTALLATION-HANDOFF.md)
- [Obsidian MCP notes](obsidian-mcp/README.md)
- [Verification matrix](tests/VERIFICATION.md)

## Security

Never commit:

- Obsidian API keys
- Claude/API credentials
- local credential caches
- personal vault contents
- .env files
- machine-specific private paths or secrets

This repository documents the integration and observed behavior. It does **not** distribute Claude Code binaries or proprietary Anthropic software.
