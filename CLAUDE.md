# Agent Instructions

## Project Purpose

This project exists to build n8n workflows. Every session is a workflow request — your job is to design, build, and deploy it into the n8n instance at high quality using the tools available to you.

---

## Available Tools

### n8n MCP Server

Provides direct access to n8n node documentation and your live n8n instance. Use it throughout every workflow build.

**Documentation & search tools:**
- `search_nodes` — search across 1,084 n8n nodes (core + community)
- `get_node_info` — retrieve full details on a specific node
- `get_node_properties` — get configuration schemas and property definitions
- `get_node_operations` — list available operations for a node
- `get_documentation` — retrieve official n8n docs for a node or concept
- `get_examples` — pull pre-extracted configs from 2,646 real workflow templates
- `get_ai_tools` — find AI-capable node variants
- `search_templates` — search 2,709 workflow templates by keyword
- `search_community_nodes` — search verified community integrations

**Workflow management tools (requires n8n API credentials):**
- `create_workflow` — build a new workflow in the instance
- `update_workflow` — modify an existing workflow
- `execute_workflow` — run a workflow to test it
- `validate_workflow` — check a workflow config for errors before deploying

**Safety rule:** Never edit production workflows directly with AI. Always build and test in a development environment first.

---

### n8n Skills

Seven Claude Code skills that provide deep n8n expertise. Invoke the relevant one(s) at the start of a build to get pattern guidance, correct syntax, and node configuration rules.

| Skill | When to use |
|---|---|
| `n8n Expression Syntax` | Any time you write expressions — covers `$json`, `$node`, `$now`, `$env`, and common mistakes. Note: webhook data is at `$json.body`, not `$json`. |
| `n8n MCP Tools Expert` | When using the MCP server — covers tool selection, nodeType formats, validation profiles |
| `n8n Workflow Patterns` | Before designing a workflow — 5 proven patterns: webhook processing, HTTP API, database, AI agent, scheduled |
| `n8n Validation Expert` | When `validate_workflow` returns errors — interprets errors, explains auto-sanitization, guides fixes |
| `n8n Node Configuration` | When configuring complex nodes — covers property dependency rules (e.g. `sendBody → contentType`), AI connection types |
| `n8n Code JavaScript` | When writing JavaScript Code nodes — data access patterns, built-in functions, top error scenarios |
| `n8n Code Python` | When writing Python Code nodes — covers critical limitation: no external libraries available |

---

## How to Approach a Workflow Request

1. **Clarify before building** — If the trigger, inputs, outputs, or failure behavior are unclear, ask. Don't guess.
2. **Check what exists** — Use `search_nodes` or `search_templates` to find existing patterns. Check the live instance before creating anything new.
3. **Invoke the right skills** — Use `n8n Workflow Patterns` to pick an architecture. Use `n8n Node Configuration` and `n8n Expression Syntax` as you build.
4. **Design before executing** — Outline the trigger, main logic path, and error branch before calling `create_workflow`.
5. **Validate, then deploy** — Run `validate_workflow` before activating. Use `execute_workflow` to verify it behaves correctly.

---

## Quality Standards

### Documentation
- Workflow names use the format: **verb + subject** (e.g. "Sync Contacts to Mailchimp", "Send Weekly Report Email")
- Node names describe what they do, not their type — "Get Active Contacts" not "HTTP Request"
- Add sticky notes to explain: non-obvious logic, required credentials, trigger conditions, known limitations

### Error Handling
- All HTTP Request nodes and integration nodes get an error branch
- Error branches must log or notify — never silently discard failures
- For critical automations, configure the n8n built-in error workflow setting

### Performance
- Prefer batch operations over per-item API calls when the service supports it
- Use a single Set node or Code node for simple transforms instead of chaining multiple nodes
- Avoid intermediate nodes that don't meaningfully transform or route data

---

## What NOT to Do

- Don't start building without understanding the trigger and end goal
- Don't skip error handling, even for workflows that seem simple
- Don't create a new workflow if an existing one can be extended
- Don't hardcode credentials — always use n8n's credential system
- Don't edit the production n8n instance without testing in development first
