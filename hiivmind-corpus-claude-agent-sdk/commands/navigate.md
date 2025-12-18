---
description: Ask questions about Claude Agent SDK documentation or manage this corpus
argument-hint: Question or command (e.g., "how do custom tools work?", "refresh", "enhance")
allowed-tools: ["Read", "Grep", "Glob", "WebFetch", "AskUserQuestion", "Skill", "Task", "TodoWrite"]
---

# Claude Agent SDK Corpus Navigator

Direct access to Claude Agent SDK documentation.

**User request:** $ARGUMENTS

**Corpus location:** `${CLAUDE_PLUGIN_ROOT}`

---

## Routing

### If no arguments provided

Show a brief help message:

```
Claude Agent SDK Corpus - ask me anything!

Examples:
  /hiivmind-corpus-claude-agent-sdk:navigate how do custom tools work?
  /hiivmind-corpus-claude-agent-sdk:navigate TypeScript SDK setup
  /hiivmind-corpus-claude-agent-sdk:navigate agent configuration

Maintenance:
  /hiivmind-corpus-claude-agent-sdk:navigate refresh     - Update index from upstream
  /hiivmind-corpus-claude-agent-sdk:navigate enhance X   - Add depth to topic X
  /hiivmind-corpus-claude-agent-sdk:navigate status      - Check corpus freshness
```

### If arguments match a maintenance command

| Pattern | Skill to Load |
|---------|---------------|
| `refresh`, `update`, `sync` | `hiivmind-corpus-refresh` |
| `enhance {topic}` | `hiivmind-corpus-enhance` |
| `status`, `freshness`, `stale?` | `hiivmind-corpus-refresh` (status mode) |
| `add source {url}` | `hiivmind-corpus-add-source` |
| `awareness`, `inject`, `claude.md` | `hiivmind-corpus-awareness` |

**How to invoke parent skills:**

1. **Set corpus context** - All paths are relative to `${CLAUDE_PLUGIN_ROOT}`
2. **Load the skill** via the Skill tool (e.g., `hiivmind-corpus-refresh`)
3. **Pass context** in your message: "Working in Claude Agent SDK corpus at ${CLAUDE_PLUGIN_ROOT}. User wants to {action}."

The parent skill will then operate on THIS corpus using relative paths like `data/config.yaml`, `data/index.md`, `.cache/`, etc.

### Otherwise: Navigate (default path)

This is a documentation question. Follow the navigation process below.

---

## Navigation Process

1. **Search the indexes first** (see Index Search section below)
   - Extract key concepts from the question
   - Grep the index files for those terms
   - If matches found → use those entries directly
2. **If no search matches, read the index**: `data/index.md`
3. **Parse path format**: `{source_id}:{relative_path}`
4. **Look up source** in `data/config.yaml` by ID
5. **Get content** based on source type (see Source Access below)
6. **Answer** with citation to source and file path

---

## Index Search (Recommended First Step)

**Before reading sections sequentially, SEARCH the indexes for relevant terms.**

### Step 1: Extract Key Concepts

Analyze the user's question and extract:
- **Explicit terms**: Words directly used in the question
- **Synonyms**: Related terms (e.g., "tool" → also try "function", "capability")
- **Domain terms**: Technical concepts that might appear in docs (e.g., "agent" → also try "SDK", "client")

### Step 2: Search Index Files

```bash
# Search for each term in the index
grep -ri "{term}" data/ --include="*.md"
```

**Tip:** Search for the most specific term first. If no matches, broaden to synonyms.

### Step 3: Use Search Results

If grep finds matches:
1. **Direct hit**: Entry description matches the question → use that path
2. **Related hit**: Entry is in the right area but doesn't directly answer → **GO TO STEP 4 IMMEDIATELY**
3. **No hit in index**: The topic may not be indexed → **GO TO STEP 4 IMMEDIATELY**

**CRITICAL: Use the EXACT path from the index! NEVER guess or invent filenames.**

### Step 4: Clarify Before Exploring (IMPORTANT!)

**STOP HERE if you only found related content, not a direct answer.**

Don't start exploring, guessing paths, or searching the source files. Instead, **use AskUserQuestion** to clarify:

**Present these options:**

1. **Clarify the request** - Maybe the question can be rephrased
2. **Show what's available** - Help the user find related content
3. **Identify an index gap** - If the user confirms their request was valid
   - Offer next steps:
     - `/hiivmind-corpus-claude-agent-sdk:navigate enhance {topic}` - Add depth
     - `/hiivmind-corpus-claude-agent-sdk:navigate add source {url}` - Add another source
     - `/hiivmind-corpus-claude-agent-sdk:navigate refresh` - Check for upstream updates

---

## Path Format

Index entries use the format: `{source_id}:{relative_path}`

Examples:
- `agent-sdk:overview.md` - Web source
- `agent-sdk:typescript.md` - Web source
- `agent-sdk:custom-tools.md` - Web source

---

## Source Access

### Worked Example (IMPORTANT - Follow This Pattern!)

**Index entry found:** `agent-sdk:custom-tools.md`

**Step 1 - Parse the path:**
- `source_id` = `agent-sdk` (everything before the colon)
- `relative_path` = `custom-tools.md` (everything after the colon)

**Step 2 - Check cache first:**
Look for: `.cache/web/agent-sdk/custom-tools.md`

**Step 3 - If cache miss, look up URL in config.yaml:**
```yaml
sources:
  - id: agent-sdk
    type: web
    urls:
      - url: https://docs.anthropic.com/en/docs/agents-and-tools/claude-agent-sdk/custom-tools
        cached_as: custom-tools.md
```

**CRITICAL:** The `relative_path` from the index is used EXACTLY as-is. NEVER invent or guess filenames!

### For web sources

Read from cache: `.cache/web/{source_id}/{relative_path}`

If cache miss, look up the URL in `data/config.yaml` and fetch fresh content using WebFetch.

### For git sources

**If `.source/{source_id}/` exists (local clone):**
Read directly from `.source/{source_id}/{docs_root}/{relative_path}`

**If no local clone:**
Fetch from GitHub using the EXACT path from the index.

### For local sources

Read directly from: `data/uploads/{source_id}/{path}`

---

## File Locations

- **Index**: `data/index.md`
- **Config**: `data/config.yaml` (has sources array with per-source tracking)
- **Git sources**: `.source/{source_id}/` (cloned repos, gitignored)
- **Local sources**: `data/uploads/{source_id}/` (user-uploaded files)
- **Web cache**: `.cache/web/{source_id}/` (fetched web content, gitignored)

---

## Output

- Cite the source ID and file path for reference
- Include code examples from the docs
- Suggest related docs from the same index section
- Note source type and freshness warnings if relevant

---

## Making Projects Aware of This Corpus

If you're working in a project that uses the Claude Agent SDK but doesn't know about this corpus, you can add awareness to the project's CLAUDE.md.

**The `references/project-awareness.md` file** contains a ready-to-use snippet.

### When to Suggest Injection

Suggest adding project awareness when:
- User is working in a project that uses the Claude Agent SDK
- User repeatedly asks Agent SDK questions without invoking the corpus
- User says "I keep forgetting to use the docs"

---

## Example Sessions

### Documentation Question

**User:** `/hiivmind-corpus-claude-agent-sdk:navigate how do custom tools work?`

1. Grep `data/index.md` for "custom tools"
2. Find entry like `agent-sdk:custom-tools.md`
3. Read from `.cache/web/agent-sdk/custom-tools.md`
4. Answer with code examples and link to source

### Maintenance: Refresh

**User:** `/hiivmind-corpus-claude-agent-sdk:navigate refresh`

1. Detect maintenance keyword: `refresh`
2. Load parent skill: `hiivmind-corpus-refresh`
3. Context message: "Working in Claude Agent SDK corpus at ${CLAUDE_PLUGIN_ROOT}. User wants to refresh/sync from upstream."
4. Parent skill executes using relative paths

### Maintenance: Enhance

**User:** `/hiivmind-corpus-claude-agent-sdk:navigate enhance tools`

1. Detect maintenance keyword: `enhance`
2. Extract topic: `tools`
3. Load parent skill: `hiivmind-corpus-enhance`
4. Context message: "Working in Claude Agent SDK corpus at ${CLAUDE_PLUGIN_ROOT}. User wants to enhance the 'tools' section."
