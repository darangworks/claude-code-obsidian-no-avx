# Verification Matrix

| Test | Result |
|---|---|
| Claude Code native Node.js bundle generated | VERIFIED |
| CLI starts under Node.js 24 | VERIFIED |
| Claude Code MCP stdio | VERIFIED |
| Obsidian REST connectivity | VERIFIED |
| Vault discovery | VERIFIED |
| Note read | VERIFIED |
| Vault search | VERIFIED |
| Large-note JSON read | VERIFIED |
| Large-note Markdown read | FAILED — HTTP 500 |
| Write note | NOT TESTED |
| Claude authentication | NOT VERIFIED |
| Built-in plugin hooks | NOT VERIFIED |

## Interpretation

**VERIFIED** means directly observed during the installation/testing workflow.

**NOT TESTED** means no evidence was collected.

**NOT VERIFIED** means the component may work, but the available evidence does not establish it.

**FAILED** records an observed failure and must not be silently converted into a success claim.
