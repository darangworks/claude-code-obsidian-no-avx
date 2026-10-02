# Running Claude Code on an Old PC Without AVX or AVX2

I have a computer with an Intel Core 2 Duo E8400, 4 GB of RAM, and no AVX or AVX2 support.

On hardware like this, running newer software can become a technical problem. Claude Code was one of those cases. The official Bun-based build did not run on this CPU and crashed at startup.

Instead of giving up on the machine, I decided to test a specific question:

**Can Claude Code be run in a way that does not depend on AVX or AVX2?**

I documented the result as an independent GitHub project.

## Test Hardware and Environment

The test environment was intentionally an old machine:

- CPU: Intel Core 2 Duo E8400
- OS: Windows 10 Pro x64
- RAM: 4 GB
- AVX/AVX2: unavailable
- Node.js: 24.19.0

These details matter because this experiment should not be generalized to every CPU without AVX. The evidence applies to the configuration that was actually tested.

## The Workaround

The route I tested was `cc2js`.

The idea is to convert the Bun-compiled Claude Code executable into a Node.js-compatible bundle.

The tested versions were:

- Claude Code `2.1.270`
- `cc2js` `1.2.3`
- Node.js `24.19.0`

The conversion completed successfully and produced the Node.js bundle.

But generating the bundle was not enough. I also ran basic checks:

```text
node cli.js --help
node cli.js mcp --help
node cli.js doctor
```

The CLI started successfully and these checks passed.

I also performed a separate MCP test using the `stdio` transport. MCP connectivity was confirmed at this basic transport level.

So, within the scope of this experiment:

**The Node.js-compatible bundle ran on the tested machine, and basic MCP stdio connectivity was verified.**

## Then I Tested the Obsidian Path

The second part of the project involved Obsidian.

There is an important boundary here:

**I did not verify a complete Claude Code → Obsidian workflow in this release.**

Instead, I tested the Obsidian path separately.

The tested architecture was:

```text
Claude Desktop / MCP Client
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

The following operations were successfully tested:

- Vault discovery
- Reading a specific note
- Searching vault content
- Reading a note as JSON

So the Obsidian REST/MCP path was usable within the scope of the recorded tests.

## A Technical Detail About HTTPS

During the project, an ambiguity came up around the Obsidian endpoint.

Instead of guessing, I inspected the bridge source used in the test.

The tested `obsidian-mcp-rest` release supports a configurable `baseUrl`. The following endpoint was therefore used directly in the experiment:

```text
https://127.0.0.1:27124
```

No TLS proxy or bridge source patch was required for this path.

The repository records this detail so that the evidence behind the endpoint choice is explicit.

## Not Everything Worked

One of the important findings was a failure.

When I tried to read a large Research Card as complete Markdown, the request returned:

```text
HTTP 500
errorCode: 50000
```

I did not establish the cause.

That was deliberate. If the cause of a failure has not been tested, it should not be presented as a fact.

The project therefore records this case as **FAILED**, with the cause remaining **NOT ESTABLISHED**.

## What Remains Unverified?

In release `v1.0.0`, several areas remain open:

- Claude Code authentication and real session behavior
- Long-running Claude Code stability
- Full Claude Code → Obsidian integration
- Obsidian write, edit, and delete operations
- Audio capture
- Automatic update compatibility
- Full built-in plugin compatibility
- A comprehensive CPU-instruction audit of native dependencies

These are intentionally separated in the documentation so that “not tested” is not confused with “does not work.”

## What Does This Project Actually Show?

This project does not establish:

> Claude Code runs on all CPUs without AVX and AVX2.

A single test machine cannot establish that.

The narrower conclusion is:

> On the tested environment, using an Intel Core 2 Duo E8400 and Windows 10, I converted Claude Code 2.1.270 with `cc2js` into a Node.js-compatible bundle and verified CLI execution and basic MCP connectivity.

For Obsidian:

> The Obsidian MCP/REST path was tested independently and several read operations succeeded, but the complete Claude Code → Obsidian workflow was not verified in this release.

That distinction matters to me.

## Why Publish the Project?

The goal was not just to find a workaround.

I wanted the result to be inspectable: which parts were actually tested, which parts remain open, and where the experiment failed.

The repository therefore includes the test environment, conversion method, verification steps, Obsidian architecture, tool versions, and known limitations.

Release `v1.0.0` was published after a final audit and recorded as the project's **Evidence Freeze**.

## What I Learned

For me, the most interesting part of the project was not simply getting Claude Code to run on an old CPU.

The more important lesson was keeping a clear boundary between **evidence and assumptions**.

If something was not tested, say so.

If a test failed, record the failure.

If the cause of a failure is unknown, do not invent one.

That is why this project uses explicit status labels:

```text
VERIFIED
FAILED
NOT VERIFIED
NOT TESTED
NOT ESTABLISHED
```

Each one means something different.

That distinction may be more valuable than the workaround itself.

## Published Version

Release `v1.0.0` is published, with the experiment report, reproduction documentation, and verification matrix included in the repository.

For now, this project is a **documented technical case study**, not a general claim that Claude Code is compatible with all legacy hardware.

If further experiments are performed, they should be recorded as new evidence outside the Evidence Freeze represented by `v1.0.0`.

[نسخهٔ فارسی](claude-code-no-avx-fa.md)

[Repository](https://github.com/darangworks/claude-code-obsidian-no-avx)
