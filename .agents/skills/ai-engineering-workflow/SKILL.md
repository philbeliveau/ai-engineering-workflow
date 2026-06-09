---
name: ai-engineering-workflow
description: Start or continue this repo's AI Engineering Workflow. Use when the user asks for the workflow, AI engineering process, BMAD workflow, project sidecar, or step-by-step LLM system design.
---

Load `workflow.md` completely before taking action.

Follow the workflow rules exactly:

1. Initialize or continue the project using the `sidecar.yaml` state described in `workflow.md`.
2. Execute only the current step file.
3. Load the matching agent persona from `agents/` when the step requires it.
4. Query Knowledge MCP at designated decision points.
5. Persist outputs and sidecar state before continuing.
6. Stop at menus and wait for the user's choice.

For direct step execution, use the relevant step file under `steps/`.
