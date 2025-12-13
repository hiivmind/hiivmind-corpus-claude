---
name: hiivmind-corpus-claude-agent-sdk-navigate
description: Find relevant Claude Agent SDK documentation. Use when working with Claude Agent SDK code, APIs, or configuration.
---

# Claude Agent SDK Corpus Navigator

Find and retrieve relevant documentation from the Claude Agent SDK corpus.

## Process

1. **Search the indexes first** (see Index Search section below)
   - Extract key concepts from the question
   - Grep the index files for those terms
   - If matches found → use those entries directly
2. **If no search matches, read the index**: `data/index.md`
3. **Parse path format**: `{source_id}:{relative_path}`
4. **Look up source** in `data/config.yaml` by ID
5. **Get content** based on source type (see Source Access below)
6. **Answer** with citation to source and file path

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
2. **Related hit**: Entry is in the right area → read that section for context
3. **No hit in index**: The topic may not be indexed yet → report this as an index gap

### Step 4: Report Index Gaps

If the user's question cannot be answered because the index lacks relevant entries:

> "The index doesn't have entries for '{term}'. The closest match is '{related_section}'.
> If this topic should be covered, consider running `hiivmind-corpus-enhance` to add depth to this area."

## Path Format

Index entries use the format: `{source_id}:{relative_path}`

Examples:
- `agent-sdk:overview.md` - Web source
- `agent-sdk:typescript.md` - Web source
- `agent-sdk:custom-tools.md` - Web source

## Source Access

### For web sources

Read from cache: `.cache/web/{source_id}/{cached_file}`

If cache miss, look up the URL in `data/config.yaml` and fetch fresh content using WebFetch.

**Cache structure:**
- `.cache/web/agent-sdk/overview.md`
- `.cache/web/agent-sdk/typescript.md`
- etc.

### For git sources

Look up the source in `data/config.yaml` to get `repo_owner`, `repo_name`, `branch`, and `docs_root`.

**IMPORTANT: Always check for local clone first!**

**Step 1 - Check for local clone:**
Use Glob or Bash to check if `.source/{source_id}/` exists.

**Step 2a - If local clone exists (PREFERRED):**
Read directly from `.source/{source_id}/{docs_root}/{path}` using the Read tool.
This is faster and works offline.

**Step 2b - If NO local clone exists (fallback only):**
Fetch from GitHub:
```
https://raw.githubusercontent.com/{repo_owner}/{repo_name}/{branch}/{docs_root}/{path}
```
Use WebFetch to retrieve content.

### For local sources

Read directly from: `data/uploads/{source_id}/{path}`

Local sources are user-uploaded files stored within the corpus.

## File Locations

- **Index**: `data/index.md`
- **Config**: `data/config.yaml` (has sources array with per-source tracking)
- **Git sources**: `.source/{source_id}/` (cloned repos, gitignored)
- **Local sources**: `data/uploads/{source_id}/` (user-uploaded files)
- **Web cache**: `.cache/web/{source_id}/` (fetched web content, gitignored)

## Output

- Cite the source ID and file path for reference
- Include code examples from the docs
- Suggest related docs from the same index section
- Note source type and freshness warnings if relevant

## Making Projects Aware of This Corpus

If you're working in a project that uses the Claude Agent SDK but doesn't know about this corpus, you can add awareness to the project's CLAUDE.md.

**The `data/project-awareness.md` file** contains a ready-to-use snippet that can be added to any project's CLAUDE.md to make Claude aware of this corpus when working in that project.

### How to Inject

1. Read `data/project-awareness.md` from this corpus
2. Add its contents to the target project's CLAUDE.md (create if needed)
3. The project will now know to use this corpus for Claude Agent SDK questions

### When to Suggest Injection

Suggest adding project awareness when:
- User is working in a project that uses the Claude Agent SDK
- User repeatedly asks Agent SDK questions without invoking the corpus
- User says "I keep forgetting to use the docs"

Example suggestion:
> "I notice this project uses the Claude Agent SDK. Would you like me to add corpus awareness to this project's CLAUDE.md? That way I'll automatically know to check the Agent SDK docs when working here."
