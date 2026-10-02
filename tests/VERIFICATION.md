# Verification Matrix

The matrix intentionally scopes each result to the layer and client that were actually tested.

| Test | Client / path | Result | Evidence / limitation |
|---|---|---|---|
| Node.js Claude Code bundle generated | cc2js → Node.js | VERIFIED | Claude Code 2.1.270 converted with cc2js 1.2.3 |
| CLI starts under Node.js 24 | Node.js | VERIFIED | `node .\cli.js --help` |
| CLI diagnostics | Node.js | VERIFIED | `node .\cli.js doctor` reported no installation issues |
| MCP stdio transport | Claude Code → generic Node MCP server | VERIFIED | Observed `Status: Connected`, `Type: stdio`, `Command: node` |
| Claude Code → Obsidian | Claude Code end-to-end | NOT VERIFIED | No end-to-end test was performed |
| Obsidian REST connectivity | Claude Desktop → MCPB → Node → REST | VERIFIED | Tested local HTTPS endpoint `https://127.0.0.1:27124` |
| Vault discovery | Claude Desktop Obsidian path | VERIFIED | Read-path test |
| Specific note read | Claude Desktop Obsidian path | VERIFIED | Read-path test |
| Vault content search | Claude Desktop Obsidian path | VERIFIED | Search test |
| Large-note JSON read | Claude Desktop Obsidian path | VERIFIED | JSON representation succeeded |
| Large-note Markdown read | Claude Desktop Obsidian path | FAILED | HTTP 500 / `errorCode: 50000`; cause not established |
| Write note | Obsidian path | NOT TESTED | No create/edit/delete/move/rename test |
| Claude authentication/session | Claude Code | NOT VERIFIED | Not tested |
| Long-running interactive stability | Claude Code | NOT VERIFIED | Not tested |
| Audio capture | Claude Code | NOT VERIFIED | Not tested |
| Automatic update compatibility | Claude Code | NOT VERIFIED | Not tested |
| Built-in plugin hooks | Claude Code | NOT VERIFIED | Conversion emitted an unrecognised built-in plugin hooks warning |

## Endpoint

The successful Obsidian test used:

```text
https://127.0.0.1:27124
```

Port `27123` was not the endpoint recorded for that successful test.

## Interpretation

**VERIFIED** means directly observed during the documented workflow at the stated layer.

**NOT TESTED** means no test was performed.

**NOT VERIFIED** means the available evidence does not establish that the capability works.

**FAILED** records an observed failure. It must not be silently converted into a success claim.

A result at one layer does not establish the behavior of downstream layers.
