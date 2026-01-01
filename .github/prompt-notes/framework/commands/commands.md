## Commands

**CRITICAL**: Follow ALL instructions and guideline files precisely.  
**ALWAYS LOAD**: `${PROMPTS_DIR}/guidelines/code-style-guidelines.md` before code changes.

### Workflows
- `@plan` — Planning mode (loads `${PROMPTS_DIR}/instructions/workflows/plan-instructions.md`)
- `@build` — Implementation + verification only (loads `${PROMPTS_DIR}/instructions/workflows/build-instructions.md`)
- `@test` — Test maintenance based on git changes (loads `${PROMPTS_DIR}/instructions/workflows/test-instructions.md`)
- `@e2e` — Generate E2E test specs (loads `${PROMPTS_DIR}/instructions/workflows/e2e-instructions.md`)
- `@ui-accurate` — Pixel-perfect UI refinement (loads `${PROMPTS_DIR}/instructions/workflows/ui-accurate-instructions.md`)
- `@stress-ui` — Stress-test mock data generation (loads `${PROMPTS_DIR}/instructions/workflows/stress-ui-instructions.md`)
- `@diagram` — Mermaid architecture diagrams (loads `${PROMPTS_DIR}/instructions/workflows/diagram-instructions.md`)
- `@extract-concerns` — Concern-level duplication analyzer (loads `${PROMPTS_DIR}/instructions/workflows/extract-concerns-instructions.md`)
- `@initiative` — Proposal-only improvement scan (loads `${PROMPTS_DIR}/instructions/workflows/initiative-instructions.md`)

### Utilities
- `@deep-think` — Multi-approach analysis + scoring (loads `${PROMPTS_DIR}/util/deep-think-instructions.md`)
- `@summarize` — Summarize a prompt file in-place (loads `${PROMPTS_DIR}/util/summarize-instructions.md`)
- `@generate-spec` — Extract instruction essence into portable template (loads `${PROMPTS_DIR}/util/generate-spec-instructions.md`)
- `@generate-command` — Generate project-specific command from spec (loads `${PROMPTS_DIR}/util/generate-command-instructions.md`)
