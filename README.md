# knowledge-stack

Claude Code plugin that wraps three knowledge domains behind one routing command. The user types `/knowledge-stack:ask <question>` and the command delegates to the right specialist subagent.

## What it gives you

| Specialist | When to invoke | Backed by |
|---|---|---|
| `code-specialist` | Questions about code structure: where is symbol X defined, who calls function Y, what symbols exist in a file, cross-repo references | [scip-mcp](https://github.com/kimsb2429/scip-mcp) — cross-repo code intelligence over SCIP indexes |
| `docs-specialist` | Questions answerable from indexed document corpora (policy, manuals, written guidance) | A RAG MCP exposing a `query` tool (e.g. internal-knowledge-base) |
| `work-specialist` | Confluence pages, Jira tickets, sprint state, internal workplace knowledge | Atlassian's MCP server |

The plugin ships **only the routing layer** — three subagent descriptions, an `/ask` command, and the manifest. The underlying MCP servers are **not bundled** in this version; they must be registered separately in your Claude Code config.

## Install

This repo is both a Claude Code marketplace and a plugin (single-plugin marketplace pattern). Two commands:

```
/plugin marketplace add kimsb2429/knowledge-stack
/plugin install knowledge-stack@knowledge-stack
```

The first adds this repo as a marketplace; the second installs the plugin from it.

For local development:

```
claude --plugin-dir /path/to/knowledge-stack
```

## Required setup (per specialist)

Each specialist needs its underlying MCP server registered in your Claude Code config. **If a specialist's MCP is not registered, the specialist will say so and stop** — it will not fabricate an answer.

- **code-specialist** needs an MCP server exposing `find_definition`, `find_references`, `list_symbols_in_file`, `search_code_symbols`. Reference implementation: [scip-mcp](https://github.com/kimsb2429/scip-mcp).
- **docs-specialist** needs an MCP server exposing a `query` tool over an indexed document corpus.
- **work-specialist** needs Atlassian's MCP server (the official remote MCP or any equivalent exposing Confluence + Jira tools).

If you only have a subset registered (e.g. only Atlassian at a workplace), only the matching specialist will function. The others remain installed but inactive.

## Usage

```
/knowledge-stack:ask Where is the User class defined and is there a Confluence page documenting our auth flow?
```

The command will fan out to `code-specialist` and `work-specialist` in parallel and synthesize the answers.

## Status

**v0.0.1 — scaffold.** Routing layer wired. MCP server configs are not yet bundled in `plugin.json`; future versions will bundle them so installing the plugin auto-registers the MCPs.
