# To add the mcp
  Codex mcp add --transport sse knowledge-pipeline https://knowledge-mcp-production.up.railway.app/mcp
  
## Knowledge MCP Server

**Live:** https://knowledge-mcp-production.up.railway.app

### Available Tools

| Tool | Description | Best For |
|------|-------------|----------|
| `search_knowledge` | Semantic search across all AI engineering knowledge | General questions, finding context |
| `get_decisions` | Architectural decisions with trade-offs | "Should I use X or Y?" questions |
| `get_patterns` | Reusable implementation patterns with code | "How do I implement X?" questions |
| `get_warnings` | Anti-patterns and pitfalls to avoid | Before implementing anything |
| `get_methodologies` | Step-by-step processes | Workflow guidance |
| `list_sources` | List all knowledge sources (58 docs) | Discovering available content |
| `compare_sources` | Compare how different sources address a topic | Multiple perspectives |

### How to Query the Knowledge MCP

**Method 1: Slash Commands** (if MCP connected via SSE and your client exposes BMAD slash commands)
```
/bmad:knowledge:search-knowledge <query>
/bmad:knowledge:get-patterns <topic>
/bmad:knowledge:get-warnings <topic>
/bmad:knowledge:get-decisions <topic>
/bmad:knowledge:list-sources
```

**Method 2: Codex MCP Tool Requests**

When using Codex, ask Codex to use the connected Knowledge MCP tool by name:

```text
Use search_knowledge for "RAG chunking strategy for legal documents"
Use get_patterns for topic "chunking"
Use get_warnings for topic "rag"
Use get_decisions for topic "rag vs fine-tuning"
Use list_sources
```

If the MCP tools are not exposed in the current Codex session, use Direct HTTP.

**Method 3: Direct HTTP (always works)**

```bash
# Semantic search (POST)
curl -s -X POST "https://knowledge-mcp-production.up.railway.app/search_knowledge?query=YOUR_QUERY&limit=10"

# Get patterns (GET)
curl -s "https://knowledge-mcp-production.up.railway.app/get_patterns?topic=YOUR_TOPIC"

# Get warnings (GET)
curl -s "https://knowledge-mcp-production.up.railway.app/get_warnings?topic=YOUR_TOPIC"

# Get decisions (GET)
curl -s "https://knowledge-mcp-production.up.railway.app/get_decisions?topic=YOUR_TOPIC"

# List sources (GET)
curl -s "https://knowledge-mcp-production.up.railway.app/list_sources"

# Health check
curl -s "https://knowledge-mcp-production.up.railway.app/health"
```

### Query Tips

**For `search_knowledge`:**
- Be specific: "embedding dimension trade-offs" not just "embeddings"
- Include domain context: "RAG chunking" not just "chunking"
- Use technical terms: "vector similarity" not "finding similar things"
- Make 2-3 calls with varied phrasings for comprehensive answers

**For `get_patterns`:**
- Topic filters: `rag`, `embeddings`, `chunking`, `llm-ops`, `fine-tuning`, `prompt-engineering`
- Returns: Pattern name, problem, solution, code examples, when to use

**For `get_warnings`:**
- ALWAYS query before implementing anything
- Returns: Warning, symptoms, consequences, prevention, mitigation

**For `get_decisions`:**
- Use when choosing between approaches
- Returns: Question, options, considerations, recommended approach

### Example Queries

```bash
# Find MongoDB + Vector DB patterns
curl -s -X POST "https://knowledge-mcp-production.up.railway.app/search_knowledge?query=NoSQL%20MongoDB%20document%20store%20vector%20database&limit=10"

# Get RAG warnings
curl -s "https://knowledge-mcp-production.up.railway.app/get_warnings?topic=rag"

# Get chunking patterns
curl -s "https://knowledge-mcp-production.up.railway.app/get_patterns?topic=chunking"

# RAG vs fine-tuning decisions
curl -s "https://knowledge-mcp-production.up.railway.app/get_decisions?topic=rag"
```

### Knowledge Sources Available

The database contains **58 documents** including:
- **LLM Handbook** - Comprehensive LLM engineering guide
- **LLMs in Production** - Production deployment patterns
- **AI Engineering** (Chip Huyen, 2024) - Industry best practices
- **Agentic RAG Systems** - RAG architecture patterns
- **40+ research papers** - Prompt engineering techniques

### MCP Config (Optional)

Add to `.mcp.json` for SSE connection:
```json
{
  "mcpServers": {
    "knowledge-pipeline": {
      "type": "sse",
      "url": "https://knowledge-mcp-production.up.railway.app/mcp"
    }
  }
}
```

**Note:** If MCP tools aren't available in session, use the curl method above.

## Codex Workflow Commands

Codex discovers repo-scoped skills from `.agents/skills`. After reloading the Codex session, type `/skills` or `$agents` and these skills should be available:

| Skill | Purpose |
|-------|---------|
| `ai-engineering-workflow` | Start or continue the full AI Engineering Workflow |
| `agents-business-analyst` | Activate the Business Analyst agent |
| `agents-fti-architect` | Activate the FTI Architect agent |
| `agents-data-engineer` | Activate the Data Engineer agent |
| `agents-embeddings-engineer` | Activate the Embeddings Engineer agent |
| `agents-fine-tuning-specialist` | Activate the Fine-Tuning Specialist agent |
| `agents-rag-specialist` | Activate the RAG Specialist agent |
| `agents-prompt-engineer` | Activate the Prompt Engineer agent |
| `agents-llm-evaluator` | Activate the LLM Evaluator agent |
| `agents-mlops-engineer` | Activate the MLOps Engineer agent |
| `agents-tech-lead` | Activate the Tech Lead agent |
| `agents-dev` | Activate the Dev agent |
| `knowledge-pipeline` | Query the Knowledge MCP |

Codex does not need Claude/BMAD slash commands to operate this workflow. Use skills when visible, or natural-language commands that name the same agent file, step file, or menu command.

### Start or Continue the Workflow

```text
Load workflow.md and start Step 1 for project <project-name>.
Load steps/0-scoping/step-01-business-analyst.md and follow it exactly.
Continue the workflow from sidecar.yaml.
```

### Step Menu Inputs

When a step presents a menu, Codex should accept the same letter choices shown in the step:

| Input | Meaning |
|-------|---------|
| `[R]` | Review the current decision or artifact |
| `[A]` | Analyze or elicit further |
| `[Q]` | Re-query Knowledge MCP with different constraints |
| `[P]` | View progress |
| `[C]` | Continue to the next step |
| `[H]` | Handoff to the dev agent |
| `[D]` | Done or dismiss |

You can type the letter or a natural-language equivalent, for example:

```text
C
Continue to the next step.
Q: Re-query Knowledge MCP for lower-cost vector database options.
```

### Tech Lead Agent Commands

Codex activation:

```text
Load agents/tech-lead.md and agents/config/tech-lead-agent.xml. Activate Marcus and show the menu.
```

Marcus accepts these commands in Codex:

| Command | Codex request |
|---------|---------------|
| `*menu` | Show Marcus's menu again |
| `*review-backlog` | Review accumulated stories from all workflow steps |
| `*validate-consistency` | Check conflicts, gaps, and inconsistencies |
| `*sequence-stories` | Sequence stories by dependency and implementation order |
| `*go-decision` | Make the GO/REVISE implementation decision |
| `*revise` | Create revision feedback for a specific step |
| `*party-mode` | Load the BMB party-mode workflow if available |
| `*advanced-elicitation` | Load the BMB advanced elicitation task if available |
| `*dismiss` | Exit the Tech Lead agent |

### Dev Agent Commands

Codex equivalent for the dev handoff:

```text
Load agents/dev.md and agents/config/dev-agent.xml. Activate Amelia and show the menu.
```

Amelia accepts these commands in Codex:

| Command | Codex request |
|---------|---------------|
| `*menu` | Show Amelia's menu again |
| `*dev-story` | Execute the next `ready-for-dev` story using red-green-refactor |
| `*code-review` | Review the story currently in `review` status |
| `*list-stories` | List all stories and statuses from `sprint-status.yaml` |
| `*party-mode` | Load the BMB party-mode workflow if available |
| `*dismiss` | Exit the Dev agent |
