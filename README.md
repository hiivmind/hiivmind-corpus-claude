# hiivmind-corpus-claude

A Claude Code plugin marketplace providing always-current documentation access for Claude-related projects.

## Corpus Plugins

| Plugin | Description |
|--------|-------------|
| [hiivmind-corpus-claude-agent-sdk](./hiivmind-corpus-claude-agent-sdk/) | Claude Agent SDK documentation |

## Installation

```bash
# Add this marketplace
/plugin marketplace add hiivmind/hiivmind-corpus-claude

# Install individual plugins
/plugin install hiivmind-corpus-claude-agent-sdk
```

## Maintenance

This marketplace is managed by **hiivmind-corpus** skills:

| Skill | Purpose |
|-------|---------|
| `hiivmind-corpus-init` | Add a new corpus to this marketplace |
| `hiivmind-corpus-build` | Build/rebuild a corpus index |
| `hiivmind-corpus-enhance` | Deepen coverage on specific topics |
| `hiivmind-corpus-refresh` | Update index from upstream changes |

### Adding a New Corpus

Run `hiivmind-corpus-init` from this directory - it will detect the existing marketplace and create a new corpus plugin as a subdirectory.

### Refreshing Documentation

```
Refresh all corpus indexes from upstream
```

## Requirements

- `hiivmind-corpus` meta-plugin installed
- Claude Code with plugin support

## License

MIT
