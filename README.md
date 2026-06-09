# Knowledge Pipeline

**A cognitive framework for building AI applications — specialized agents guide you through structured workflows, backed by contextual knowledge search across the AI engineering literature.**

<p align="center">
  <img src="assets/hero-image.png" alt="Knowledge Pipeline - Agents guiding chaos into clarity" width="600">
</p>

---

## Why This Exists

Building AI applications is overwhelming. You face hundreds of decisions:

- *"What chunking strategy for my domain? What size? Semantic or fixed?"*
- *"RAG or fine-tuning? Hybrid? What are the trade-offs for my scale?"*
- *"What embedding model? What vector DB? How do I evaluate quality?"*

The answers exist — scattered across books, papers, and case studies. But finding and synthesizing them while holding your specific context in mind is exhausting.

**Knowledge Pipeline is a cognitive framework that carries this load for you.**

It combines two key ideas:
1. **Knowledge Extraction** — Structured extractions from AI engineering literature (decisions, patterns, warnings, methodologies)
2. **Agentic Workflows** — Specialized agents that guide you through building production LLM systems with contextual, knowledge-grounded decisions at each step

---

## What's Encoded in the Knowledge Base

We extract **actionable structure** from the literature:

| Extraction Type | What It Captures |
|-----------------|------------------|
| **Decisions** | Architectural choices with trade-offs and recommendations |
| **Patterns** | Reusable implementations with context and constraints |
| **Warnings** | Anti-patterns, pitfalls, and failure modes to avoid |
| **Methodologies** | Step-by-step processes for complex tasks |

---

## How It Works

```
┌──────────────────────────────────────────────────────────────────────────┐
│  1. KNOWLEDGE EXTRACTION (One-time)                                      │
│                                                                          │
│  Books, Papers, Case Studies                                             │
│        ↓                                                                 │
│  PDF/Markdown Parsing → Semantic Chunking → Claude API Extraction        │
│        ↓                                                                 │
│  Structured prompts extract: decisions, patterns, warnings,              │
│  methodologies, agent definitions (with validation & deduplication)      │
└──────────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌──────────────────────────────────────────────────────────────────────────┐
│  2. VECTOR STORAGE                                                       │
│                                                                          │
│  MongoDB (metadata, full content) + Qdrant (768d nomic embeddings)       │
│                                                                          │
│  Semantic search with 8K context window for long-form retrieval          │
└──────────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌──────────────────────────────────────────────────────────────────────────┐
│  3. MCP SERVER (Real-time queries)                                       │
│                                                                          │
│  5 tools: search, decisions, patterns, warnings, sources                 │
│  Guides Claude to multi-query, cross-reference, and synthesize           │
└──────────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌──────────────────────────────────────────────────────────────────────────┐
│  4. AGENTIC WORKFLOWS (Contextual guidance)                              │
│                                                                          │
│  Specialized agents → Structured steps → Contextual MCP queries          │
│                                                                          │
│  Each step loads prior context → Searches are domain-aware               │
│  Questions enforce structure → Solution space shrinks                    │
│  Warnings surface proactively → Mistakes avoided                         │
└──────────────────────────────────────────────────────────────────────────┘
```

**Design Philosophy:**
- Extractions are for *navigation*, Claude is for *synthesis*
- Workflows provide *structure*, agents provide *expertise*
- Context flows forward, each step builds on the last

---

## Example: How Structure Guides Decisions

Without structure, a user asking "Should I use RAG or fine-tuning?" faces overwhelming options and trade-offs.

**With this workflow, structure shrinks the solution space:**

### Step 2A: Architecture Decision (Funnel: 100+ options → 3 paths)

**1. Build vs Buy (3-question decision tree):**
```
Q1: Is the LLM critical to your business?
├─ YES → Q2: Do you have sufficient training data?
│  ├─ YES → Q3: Privacy/security requirements?
│  │  ├─ YES → BUILD (BUILDING path)
│  │  └─ NO  → Analyze trade-offs
│  └─ NO  → RAG likely better (BUYING/RAG path)
└─ NO  → Use API access (BUYING path)
```

**2. Use Case Classification (narrows to 6 categories):**
| Use Case | Direction | Why |
|----------|-----------|-----|
| Knowledge QA | RAG | Retrieves from documents, prevents hallucinations |
| Content Generation | Fine-tuning | Needs custom style, requires training data |
| Conversational Agent | Hybrid | Needs both retrieval AND reasoning |

**3. Data Assessment (5 factors evaluate your specific situation):**
- Volume: `search_knowledge("training data volume thresholds...")`
- Quality: 1,000 high-quality samples > 50,000 low-quality
- Sensitivity: PII data? → RAG (keeps data separate)
- Update Frequency: Changes often? → RAG (always current)
- Proprietary Knowledge: Custom terminology? → Fine-tuning

**Result:** User goes from confused to confident in 15 minutes with a clear decision.

---

## The AI Engineering Workflow

### Architecture

```
┌──────────────────────────────────────────────────────────────────────────┐
│                    AI ENGINEERING WORKFLOW                               │
│                    (11 Specialized Agents)                               │
└──────────────────────────────────────────────────────────────────────────┘

PHASE 0: SCOPING
  Step 1: Business Analyst   📋  → Stakeholders, use cases, success metrics
  Step 2: FTI Architect      🏗️  → RAG vs Fine-tuning decision

PHASE 1: FEATURE PIPELINE
  Step 3: Data Engineer      🔧  → Data sources, ingestion, cleaning
  Step 4: Embeddings Engineer 🧬 → Chunking strategy, embedding model

PHASE 2: TRAINING (Conditional)
  Step 5: Fine-Tuning Specialist 🎯 → SFT/DPO config, dataset prep

PHASE 3: INFERENCE
  Step 6: RAG Specialist     🔍  → RAG pipeline, retrieval, reranking
  Step 7: Prompt Engineer    📝  → System prompts, few-shot examples

PHASE 4: EVALUATION
  Step 8: LLM Evaluator      📊  → Evaluation framework, quality gate

PHASE 5: OPERATIONS
  Step 9: MLOps Engineer     🔄  → Monitoring, drift detection, alerting

PHASE 6: INTEGRATION & HANDOFF
  Step 10: Tech Lead         👨‍💼 → Story review, sequencing, GO/REVISE decision
  Step 11: Story Elaborator  📚  → BMM format transformation, dev handoff
```

### The 11 Specialized Agents

Each agent brings domain expertise, asks clarifying questions, and generates implementation stories:

| Step | Agent | Focus |
|------|-------|-------|
| **1** | **Business Analyst** 📋 | Project initialization, stakeholder identification, use cases, success metrics |
| **2** | **FTI Architect** 🏗️ | Architecture decision (RAG-only, fine-tuning, or hybrid), design trade-offs |
| **3** | **Data Engineer** 🔧 | Data sources, ingestion pipeline, data quality, cleaning workflows |
| **4** | **Embeddings Engineer** 🧬 | Chunking strategy selection, embedding model choice, vector DB setup |
| **5** | **Fine-Tuning Specialist** 🎯 | SFT/DPO configuration, dataset preparation, training workflow (conditional) |
| **6** | **RAG Specialist** 🔍 | RAG pipeline design, retrieval strategy, reranking, context optimization |
| **7** | **Prompt Engineer** 📝 | System prompt design, few-shot examples, response templates |
| **8** | **LLM Evaluator** 📊 | Evaluation framework design, metrics selection, quality gates |
| **9** | **MLOps Engineer** 🔄 | Deployment architecture, monitoring setup, drift detection, alerting |
| **10** | **Tech Lead** 👨‍💼 | Story review, dependency sequencing, implementation validation, GO/REVISE decision |
| **11** | **Story Elaborator** 📚 | Story format transformation, task breakdown, dev handoff |

### Design Principles

**Why This Works:**
- **Micro-file Design** — Each step is self-contained, read completely before execution
- **Sequential Enforcement** — Complete phases in order, no skipping or optimization
- **Knowledge-Grounded** — Every decision references the Knowledge MCP for best practices
- **Story Generation** — Each phase outputs implementation stories (~41 total across phases)
- **State Tracking** — Progress tracked in `sidecar.yaml` with completion checkpoints
- **Conditional Routing** — RAG-only path skips Step 5; hybrid path includes it

**Why FTI Pattern:**
- Solves training-serving skew — Same feature logic in training and inference
- Clear separation of concerns — Each pipeline has one job
- Independent scaling — Scale inference without touching training
- Quality gates — Evaluation validates before operations

### How to Run the Workflow

The workflow is designed to be executed step-by-step with your AI engineering project:

1. **Start at Step 1** — Business Analyst
   - Initialize your project with stakeholder and use case discovery
   - Generate project specification and success metrics
   - Create `sidecar.yaml` for state tracking

2. **Progress Through Steps** — Follow the numbered sequence
   - Each step reads completely before execution
   - Each agent queries Knowledge MCP contextually
   - Each phase generates implementation stories

3. **Conditional Routing at Step 2** — FTI Architect decides
   - **RAG-only path** — Skips Step 5 (Training), goes directly to Step 6
   - **Fine-tuning path** — Includes Step 5, then continues to inference
   - **Hybrid path** — Uses both fine-tuning and RAG together

4. **Quality Gate at Step 8** — LLM Evaluator validates
   - Define metrics and benchmarks before inference
   - Ensure quality standards before deployment

5. **Tech Lead Review at Step 10** — Integration validation
   - Review all stories from previous steps
   - Validate sequencing and dependencies
   - Make GO/REVISE decision

6. **Handoff to Development** — Step 11 Story Elaborator
   - Transform stories to BMM format
   - Break down into implementation tasks
   - Hand off to development team via dev agent

---

## Getting Started

### Option 1: CLI Command

For Claude Code:
```bash
claude mcp add --transport sse knowledge-pipeline https://knowledge-mcp-production.up.railway.app/mcp
```

For Codex:
```bash
codex mcp add --transport sse knowledge-pipeline https://knowledge-mcp-production.up.railway.app/mcp
```

This automatically registers it and you can use it immediately from the connected agent:
```text
claude knowledge:list-sources
claude knowledge:search-knowledge "RAG chunking"
claude knowledge:get-warnings "rag"
```

In Codex, use the MCP tools when available in the session. If they are not exposed, use the direct HTTP commands below.

### Codex Agent Skills

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

Codex can also run the same workflow without explicit skill selection by loading the relevant workflow or agent file and following the menu command.

| Claude/BMAD-style command | Codex equivalent |
|---------------------------|------------------|
| `/bmad:knowledge:search-knowledge <query>` | Ask Codex: `Use search_knowledge for "<query>"` |
| `/bmad:knowledge:get-patterns <topic>` | Ask Codex: `Use get_patterns for topic "<topic>"` |
| `/bmad:knowledge:get-warnings <topic>` | Ask Codex: `Use get_warnings for topic "<topic>"` |
| `/bmad:knowledge:get-decisions <topic>` | Ask Codex: `Use get_decisions for topic "<topic>"` |
| `/bmad:knowledge:list-sources` | Ask Codex: `Use list_sources` |
| `/bmad:bmm:agents:dev` | Ask Codex: `Load agents/dev.md and agents/config/dev-agent.xml, then show Amelia's menu` |

Once an agent is loaded, Codex accepts the same menu commands:

| Agent | Commands |
|-------|----------|
| Tech Lead / Marcus | `*menu`, `*review-backlog`, `*validate-consistency`, `*sequence-stories`, `*go-decision`, `*revise`, `*party-mode`, `*advanced-elicitation`, `*dismiss` |
| Dev / Amelia | `*menu`, `*dev-story`, `*code-review`, `*list-stories`, `*party-mode`, `*dismiss` |

For normal workflow steps, Codex also accepts the letter menu choices printed by each step, such as `[R]` review, `[A]` analyze further, `[Q]` re-query Knowledge MCP, `[P]` progress, `[C]` continue, `[H]` handoff, and `[D]` done.

### Option 2: Manual Config

Add this to your Claude Desktop config or Codex `.mcp.json`:

Claude Desktop file locations:
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

Codex project file:
- `.mcp.json`

Add this config:
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

Then restart Claude Desktop or Codex.

You can now query the knowledge pipeline using these tools:
- `search_knowledge` — Semantic search across all knowledge
- `get_decisions` — Architectural decisions with trade-offs
- `get_patterns` — Reusable implementation patterns
- `get_warnings` — Anti-patterns and pitfalls to avoid
- `list_sources` — List all knowledge sources

**Example:** Ask Claude or Codex "What decisions should I consider for RAG vs fine-tuning?"


## License

MIT
