# Claude Code → Obsidian on No-AVX Windows

A documented technical case study for running a Node.js-compatible Claude Code build on legacy Windows hardware without AVX/AVX2, plus a separately verified Obsidian MCP/REST read path.

## Scope

**Tested environment**

- Windows 10 Pro x64
- Intel Core 2 Duo E8400
- No AVX/AVX2
- Node.js 24.19.0
- npm 11.x
- Git
- No WSL2, Hyper-V, or VM dependency

This repository documents the exact environment and observed results above. It does **not** establish compatibility with all no-AVX CPUs, future Claude Code releases, other Windows versions, or every native dependency.

## Architecture

### Claude Code path

```text
Claude Code
    ↓
cc2js-generated Node.js bundle
    ↓
Node.js
    ↓
MCP stdio transport
    ↓
Generic Node MCP test server
```

The generated CLI was launched under Node.js and its stdio MCP transport was tested with a generic local Node MCP server.

### Obsidian path

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

The Obsidian path was tested **separately through Claude Desktop**. This repository does **not** claim that Claude Code successfully drove the Obsidian server end-to-end.

### Endpoint clarification

The tested Obsidian endpoint was:

```text
https://127.0.0.1:27124
```

This is the encrypted HTTPS endpoint exposed by the Obsidian Local REST API configuration used during the test. Port `27123` is the alternative non-encrypted HTTP endpoint and was **not** the endpoint recorded for the successful test.

The repository does not claim that every upstream `obsidian-mcp-rest` release/configuration uses the same endpoint automatically. The tested local configuration must therefore be treated as part of the experiment.

## Quick Start

### 1. Prepare Node.js

Install Node.js 24.x and verify:

```powershell
node --version
npm --version
```

The recorded test used Node.js `24.19.0`.

### 2. Install cc2js

```powershell
npm install -g cc2js@1.2.3
cc2js --help
```

### 3. Convert the tested Claude Code executable

Use the exact Windows x64 `claude.exe` that you have legitimately obtained:

```powershell
cc2js "<path-to-claude.exe>" --no-link --out "<output-directory>"
```

Example with placeholders:

```powershell
cc2js "C:\path\to\claude.exe" `
  --no-link `
  --out "C:\path\to\claude-node"
```

The test used Claude Code `2.1.270` and produced a Node-oriented bundle containing `cli.js`, `bun-shim.cjs`, native dependencies, and generated modules.

The repository does **not** distribute the Claude Code executable or generated proprietary output.

### 4. Verify the generated CLI

From the output directory:

```powershell
node .\cli.js --help
node .\cli.js mcp --help
node .\cli.js doctor
```

### 5. Verify MCP stdio

The documented MCP test is a transport test with a generic local Node MCP server:

```text
Status: Connected
Type: stdio
Command: node
```

This establishes stdio transport connectivity. It does **not** establish arbitrary MCP-server compatibility or Obsidian integration.

### 6. Obsidian path

See:

- [Obsidian installation notes](obsidian-mcp/INSTALLATION.md)
- [Obsidian MCP notes](obsidian-mcp/README.md)
- [Reproduction guide](docs/REPRODUCTION.md)

The tested endpoint was:

```text
https://127.0.0.1:27124
```

An Obsidian API key is required locally but is intentionally excluded.

## Verification status

| Component / test | Status |
|---|---|
| Node.js Claude Code bundle generated | VERIFIED |
| Claude Code CLI launch | VERIFIED |
| CLI help / diagnostics | VERIFIED |
| MCP stdio transport with generic Node server | VERIFIED |
| Obsidian REST connectivity | VERIFIED — Claude Desktop read path |
| Vault discovery | VERIFIED — Claude Desktop read path |
| Specific note read | VERIFIED — Claude Desktop read path |
| Vault content search | VERIFIED — Claude Desktop read path |
| Large-note JSON read | VERIFIED — Claude Desktop read path |
| Large-note Markdown read | FAILED — HTTP 500 |
| Write operations | NOT TESTED |
| Claude Code authentication/session | NOT VERIFIED |
| Long-running interactive stability | NOT VERIFIED |
| Audio capture | NOT VERIFIED |
| Automatic update compatibility | NOT VERIFIED |
| Full built-in plugin compatibility | NOT VERIFIED |
| Claude Code → Obsidian end-to-end workflow | NOT VERIFIED |

See the detailed [verification matrix](tests/VERIFICATION.md).

## Known limitations

- The conversion emitted a warning that the built-in plugin hooks helper was not recognised.
- Native dependencies were not comprehensively audited for CPU-instruction requirements.
- The test demonstrates one specific no-AVX machine, not universal no-AVX compatibility.
- The tested Claude Code version was `2.1.270`; future versions are outside the evidence.
- A full plain-Markdown read of a large Research Card returned HTTP 500 / `errorCode: 50000`. The cause was not established.
- Obsidian write operations were not tested.
- Claude Code authentication/session behavior was not tested.
- The MCPB package used in the local test is not included in this repository.

## Security and licensing

Never commit:

- Obsidian API keys
- Claude/API credentials
- credential caches
- personal vault contents
- `.env` files
- generated Claude Code output
- machine-specific private paths or secrets

The MIT license in this repository applies to the repository's original documentation/configuration content. It does **not** grant rights to extracted Claude Code code, binaries, or other third-party proprietary software.

Do not paste private vault content, API keys, or credentials into GitHub issues or pull requests.

This repository does not distribute Claude Code binaries or proprietary Anthropic software.

## Documentation

- [Installation handoff](docs/INSTALLATION-HANDOFF.md)
- [Reproduction guide](docs/REPRODUCTION.md)
- [Obsidian installation](obsidian-mcp/INSTALLATION.md)
- [Obsidian MCP notes](obsidian-mcp/README.md)
- [Verification matrix](tests/VERIFICATION.md)
- [Example MCP configuration](obsidian-mcp/config.example.json)
