# Claude Agent SDK Documentation Index

> Source: platform.claude.com/docs/en/agent-sdk
> Last updated: 2025-12-12
> 20 documentation pages indexed

---

## Getting Started

### Overview
`overview` - **Agent SDK Overview**
Introduction to the Claude Agent SDK - build production AI agents with Claude Code as a library. Covers installation, authentication, core concepts, built-in tools (Read, Write, Edit, Bash, Glob, Grep, WebSearch, WebFetch), hooks, subagents, MCP integration, permissions, and sessions. Includes quickstart examples in TypeScript and Python.

**Key concepts:** query(), ClaudeAgentOptions, built-in tools, agent loop, context management
**Use when:** Starting with the SDK, understanding capabilities, first agent setup

### Migration Guide
`migration-guide` - **Migrate to Claude Agent SDK**
Guide for migrating from Claude Code SDK to Claude Agent SDK. Covers package name changes (`@anthropic-ai/claude-code` → `@anthropic-ai/claude-agent-sdk`), import updates, and breaking changes including system prompt defaults and settings sources.

**Key changes:** ClaudeCodeOptions → ClaudeAgentOptions, settingSources no longer default, system prompt no longer default
**Use when:** Upgrading from Claude Code SDK v0.0.x

---

## SDK Reference

### TypeScript SDK
`typescript` - **TypeScript SDK Reference**
Complete API reference for TypeScript. Documents all functions (`query()`, `tool()`, `createSdkMcpServer()`), types (`Options`, `Query`, `AgentDefinition`, `PermissionMode`, `McpServerConfig`), message types, hook types, tool input/output types, and sandbox configuration.

**Key functions:** query(), tool(), createSdkMcpServer()
**Key types:** Options, SDKMessage, HookEvent, ToolInput, ToolOutput
**Use when:** Building TypeScript agents, API reference lookup

### Python SDK
`python` - **Python SDK Reference**
Complete API reference for Python. Documents query() function, ClaudeSDKClient class, tool() decorator, create_sdk_mcp_server(), ClaudeAgentOptions dataclass, message types, hook types, and error handling.

**Key functions:** query(), tool(), create_sdk_mcp_server()
**Key classes:** ClaudeSDKClient, ClaudeAgentOptions
**Use when:** Building Python agents, API reference lookup

### TypeScript V2 Preview
`typescript-v2-preview` - **TypeScript V2 Interface (Preview)**
Preview of simplified V2 TypeScript SDK with session-based send/receive patterns. Removes async generator complexity for multi-turn conversations. Introduces `createSession()`, `resumeSession()`, `session.send()`, `session.receive()`.

**Key functions:** unstable_v2_createSession(), unstable_v2_resumeSession(), unstable_v2_prompt()
**Status:** Unstable preview - APIs may change
**Use when:** Simpler multi-turn conversations, evaluating V2 interface

---

## Input & Output Modes

### Streaming vs Single Mode
`streaming-vs-single-mode` - **Streaming Input**
Understanding the two input modes: Streaming Input Mode (default, recommended) for persistent interactive sessions, and Single Message Input for one-shot queries. Streaming enables image uploads, queued messages, hooks, real-time feedback.

**Streaming benefits:** Image uploads, message queueing, interruption, hooks support
**Single message use cases:** One-shot responses, stateless environments (lambdas)
**Use when:** Choosing input mode, implementing interactive sessions

### Structured Outputs
`structured-outputs` - **Structured Outputs in the SDK**
Getting validated JSON results from agent workflows using JSON Schemas. Use Zod (TypeScript) or Pydantic (Python) for type-safe schema definition. Output available in `message.structured_output`.

**Key option:** outputFormat: { type: 'json_schema', schema: {...} }
**Use when:** Need validated JSON output, type-safe agent results

---

## Tools & Extensions

### Custom Tools
`custom-tools` - **Custom Tools**
Building custom tools with `createSdkMcpServer()` and `tool()` helper. Create in-process MCP servers with type-safe tool definitions. Tool name format: `mcp__{server_name}__{tool_name}`.

**Key functions:** tool(), createSdkMcpServer()
**Use when:** Extending SDK with custom capabilities, API integrations

### MCP Integration
`mcp` - **MCP in the SDK**
Model Context Protocol servers for extending Claude Code with custom tools. Covers stdio servers (external processes), HTTP/SSE servers (remote), and SDK MCP servers (in-process). Configuration via `.mcp.json` or programmatic options.

**Transport types:** stdio, sse, http, sdk
**Use when:** Integrating external tools, databases, APIs via MCP

### Plugins
`plugins` - **Plugins in the SDK**
Loading custom plugins from local directories via `plugins` option. Plugins can include commands, agents, skills, hooks, and MCP servers. Commands namespaced as `plugin-name:command-name`.

**Key option:** plugins: [{ type: 'local', path: './my-plugin' }]
**Use when:** Loading shared extensions, team plugins

### Agent Skills
`skills` - **Agent Skills in the SDK**
Extend Claude with specialized capabilities via SKILL.md files. Skills are model-invoked based on description matching. Located in `.claude/skills/` (project) or `~/.claude/skills/` (user).

**Requirements:** settingSources includes 'project', allowedTools includes 'Skill'
**Use when:** Adding specialized capabilities, domain expertise

---

## Agents & Sessions

### Subagents
`subagents` - **Subagents in the SDK**
Working with specialized subagents for context management and parallelization. Define programmatically via `agents` option or filesystem-based in `.claude/agents/`. Benefits: separate context, parallel execution, tool restrictions.

**Key option:** agents: { 'agent-name': { description, prompt, tools?, model? } }
**Use when:** Complex workflows, parallel tasks, specialized processing

### Session Management
`sessions` - **Session Management**
Managing conversation state, session IDs, resumption, and forking. Get session ID from system init message, resume with `resume` option, fork with `forkSession: true`.

**Key options:** resume (session ID), forkSession (boolean)
**Use when:** Multi-turn workflows, persistent conversations, branching

---

## Permissions & Security

### Handling Permissions
`permissions` - **Handling Permissions**
Control tool usage via permission modes, canUseTool callback, hooks, and permission rules. Permission flow: PreToolUse Hook → Deny Rules → Allow Rules → Ask Rules → Permission Mode → canUseTool.

**Permission modes:** default, acceptEdits, bypassPermissions, plan
**Use when:** Controlling tool access, implementing approval workflows

### Hosting
`hosting` - **Hosting the Agent SDK**
Production deployment patterns for the SDK. Container-based sandboxing, resource requirements (1GiB RAM, 5GiB disk). Deployment patterns: Ephemeral sessions, long-running sessions, hybrid sessions.

**Requirements:** Node.js 18+, Claude Code CLI, container isolation
**Use when:** Production deployment planning

### Secure Deployment
`secure-deployment` - **Securely Deploying AI Agents**
Security hardening guide: isolation technologies (sandbox-runtime, containers, gVisor, VMs), credential management via proxy pattern, filesystem configuration. Defense in depth against prompt injection.

**Isolation options:** sandbox-runtime, Docker, gVisor, Firecracker VMs
**Use when:** Security hardening, multi-tenant deployments

---

## Configuration & Customization

### Modifying System Prompts
`modifying-system-prompts` - **Modifying System Prompts**
Four methods: CLAUDE.md files (project-level), output styles (persistent), systemPrompt with append, custom systemPrompt. SDK uses empty system prompt by default; use `preset: 'claude_code'` for Claude Code's prompt.

**Key options:** systemPrompt (string or { type: 'preset', preset: 'claude_code', append? })
**Use when:** Customizing agent behavior, adding project context

### Slash Commands
`slash-commands` - **Slash Commands in the SDK**
Built-in commands (/compact, /clear) and custom commands via markdown files in `.claude/commands/`. Supports arguments ($1, $2), bash execution (!`command`), file references (@filename).

**Built-in:** /compact, /clear, /help
**Custom location:** .claude/commands/*.md
**Use when:** Session control, custom workflows

---

## Monitoring & Operations

### Cost Tracking
`cost-tracking` - **Tracking Costs and Usage**
Token usage tracking for billing. Usage reported per message with same ID = same usage. Charge once per step, not per message. Result message contains cumulative `total_cost_usd`.

**Key fields:** input_tokens, output_tokens, cache_read_input_tokens, total_cost_usd
**Use when:** Implementing billing, usage monitoring

### Todo Tracking
`todo-tracking` - **Todo Lists**
Track and display task progress via TodoWrite tool. Todos have lifecycle: pending → in_progress → completed. Monitor via ToolUseBlock with name "TodoWrite".

**Tool:** TodoWrite
**Statuses:** pending, in_progress, completed
**Use when:** Progress tracking, task organization

---

## Quick Reference

### By Task

| Task | Documentation |
|------|---------------|
| First agent | `overview`, `typescript` or `python` |
| Custom tools | `custom-tools`, `mcp` |
| Multi-turn chat | `sessions`, `streaming-vs-single-mode` |
| Permissions | `permissions` |
| Production deploy | `hosting`, `secure-deployment` |
| JSON output | `structured-outputs` |
| Parallel agents | `subagents` |
| Cost billing | `cost-tracking` |

### Key Options Quick Reference

```typescript
// TypeScript
query({
  prompt: "...",
  options: {
    model: "claude-sonnet-4-5",
    allowedTools: ["Read", "Edit", "Bash"],
    permissionMode: "acceptEdits",
    systemPrompt: { type: "preset", preset: "claude_code" },
    settingSources: ["project"],
    mcpServers: { "name": serverConfig },
    agents: { "name": agentDef },
    outputFormat: { type: "json_schema", schema: {...} },
    resume: "session-id",
    maxTurns: 10
  }
})
```

```python
# Python
query(
    prompt="...",
    options=ClaudeAgentOptions(
        model="claude-sonnet-4-5",
        allowed_tools=["Read", "Edit", "Bash"],
        permission_mode="acceptEdits",
        system_prompt={"type": "preset", "preset": "claude_code"},
        setting_sources=["project"],
        mcp_servers={"name": server_config},
        agents={"name": agent_def},
        output_format={"type": "json_schema", "schema": {...}},
        resume="session-id",
        max_turns=10
    )
)
```
