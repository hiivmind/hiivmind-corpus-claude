# Claude Agent SDK Documentation Corpus

A Claude Code plugin providing always-current access to Claude Agent SDK documentation with intelligent navigation.

## Features

- **Indexed Navigation**: Structured index of all Claude Agent SDK documentation with summaries, key concepts, and cross-references
- **Automatic Updates**: Incremental index updates from upstream documentation changes
- **Smart Matching**: Finds relevant docs based on concepts, summaries, and section headings

## Installation

### As a Plugin

```bash
# Add the marketplace containing this plugin
/plugin marketplace add hiivmind/hiivmind-corpus-claude

# Install the plugin
/plugin install hiivmind-corpus-claude-agent-sdk

# Restart Claude Code
```

### Manual Installation

Clone this repository to your Claude Code plugins directory.

## Maintenance Skills

This corpus is managed by **hiivmind-corpus** skills:

| Skill | Purpose |
|-------|---------|
| `hiivmind-corpus-add-source` | Add new documentation sources |
| `hiivmind-corpus-build` | Build/rebuild the documentation index |
| `hiivmind-corpus-enhance` | Deepen coverage on specific topics |
| `hiivmind-corpus-refresh` | Update index from upstream changes |

### First-Time Setup

After installing, use the `hiivmind-corpus-build` skill to build the index:

```
Please build the Claude Agent SDK documentation index
```

This requires the `hiivmind-corpus` meta-plugin to be installed.

## Usage

### Automatic Navigation

The navigation skill is model-invoked. Simply ask questions about Claude Agent SDK:

```
How do I create custom tools in the Agent SDK?
What's the difference between streaming and single mode?
How do I handle permissions in the SDK?
How do I use subagents?
```

Claude will automatically find and cite relevant documentation.

### Index Updates

Refresh from upstream changes (uses `hiivmind-corpus-refresh`):

```
Refresh the Claude Agent SDK corpus from upstream
```

Full rebuild (uses `hiivmind-corpus-build`):

```
Rebuild the Claude Agent SDK documentation index from scratch
```

Enhance a specific topic (uses `hiivmind-corpus-enhance`):

```
Add more detail about MCP servers to the Agent SDK index
```

## Skills

### hiivmind-corpus-claude-agent-sdk-navigate

Finds relevant documentation for Claude Agent SDK-related coding tasks. Automatically invoked when you ask about:

- Agent SDK installation and setup
- TypeScript and Python SDK references
- Custom tools and MCP servers
- Subagents and plugins
- Permissions and security
- Session management
- Streaming vs single mode
- Cost and todo tracking

## Documentation Sources

| Topic | Description |
|-------|-------------|
| Overview | Introduction, installation, authentication, core concepts |
| TypeScript SDK | query(), tool(), createSdkMcpServer(), types |
| Python SDK | query(), ClaudeSDKClient, ClaudeAgentOptions |
| TypeScript V2 | Simplified V2 interface with send/receive patterns |
| Streaming vs Single | Understanding input modes |
| Migration Guide | Migrating from Claude Code SDK |
| Custom Tools | Building tools with createSdkMcpServer |
| Subagents | Programmatic and filesystem-based subagents |
| MCP | Model Context Protocol servers |
| Plugins | Loading and using custom plugins |
| Skills | Agent Skills - filesystem-based capabilities |
| Permissions | Permission modes, canUseTool, hooks |
| System Prompts | CLAUDE.md files, output styles |
| Structured Outputs | JSON schema validation |
| Sessions | Session IDs, resumption, forking |
| Hosting | Production deployment patterns |
| Secure Deployment | Security best practices |
| Slash Commands | Built-in and custom commands |
| Cost Tracking | Token usage and billing |
| Todo Tracking | TodoWrite tool |

## File Structure

```
hiivmind-corpus-claude-agent-sdk/
├── .claude-plugin/
│   └── plugin.json           # Plugin manifest
├── skills/
│   └── navigate/
│       └── SKILL.md          # Documentation navigation
├── data/
│   ├── config.yaml           # Source URLs + settings
│   └── index.md              # Generated documentation index
├── .cache/                   # Fetched web content (gitignored)
│   └── web/
└── README.md
```

## Configuration

The `data/config.yaml` and `data/index.md` files are managed by hiivmind-corpus skills. Do not edit them manually.

To add new sources, use `hiivmind-corpus-add-source`. To rebuild or enhance the index, use `hiivmind-corpus-build` or `hiivmind-corpus-enhance`.

## Requirements

- `hiivmind-corpus` meta-plugin installed (provides maintenance skills)
- Claude Code with plugin support
- Network access to platform.claude.com

## License

MIT
