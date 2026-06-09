# To add the mcp
  claude mcp add --transport sse knowledge-pipeline https://knowledge-mcp-production.up.railway.app/mcp
  
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

**Method 1: Slash Commands** (if MCP connected via SSE)
```
/bmad:knowledge:search-knowledge <query>
/bmad:knowledge:get-patterns <topic>
/bmad:knowledge:get-warnings <topic>
/bmad:knowledge:get-decisions <topic>
/bmad:knowledge:list-sources
```

**Method 2: Direct HTTP (always works)**

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
