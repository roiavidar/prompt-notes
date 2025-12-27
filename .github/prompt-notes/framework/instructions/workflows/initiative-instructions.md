# @initiative

Proactively find and propose improvements in the codebase (performance, architecture, UX, code quality).

## ⚠️ CRITICAL: NEVER IMPLEMENT ONLY CREATE PROPOSAL.md

**STOP after creating proposal.**

**DO NOT write full proposals in chat.** Write all proposal details to `${PROPOSAL_PATH}` file using create_file tool.

**Before starting**: `rm ${PROPOSAL_PATH}`

---

## TECH CONTEXT

**Stack**: React, TypeScript, Vite, MUI, tss-react, Apollo/GraphQL, Jest  
**Deep Thinking**: See `${PROMPTS_DIR}/guidelines/deep-thinking-guidelines.md` - Pattern discovery, approach exploration, scoring  
**Conventions**: See `${PROMPTS_DIR}/guidelines/code-style-guidelines.md`  
**Architecture**: See `${PROMPTS_DIR}/guidelines/architecture-guidelines.md`  
**Testing**: See `${PROMPTS_DIR}/guidelines/testing-guidelines.md`  
**Reusability**: Check `${COMMON_DIR}/`, `${UTILS_DIR}/`, `${HOOKS_DIR}/` before creating new  
**Priorities**: Stability > Usability > Performance > Accessibility > Maintainability > DX

---

## WORKFLOW

**Applies deep thinking methodology** from `${PROMPTS_DIR}/guidelines/deep-thinking-guidelines.md`

1. **Scan** — semantic_search/grep_search for patterns, violations, bottlenecks
2. **Analyze** — Evidence (file:lines), metrics, convention violations
3. **Options** — 2-3 approaches with Pros/Cons/Complexity/Reusability (use scoring formula)
4. **Recommend** — Best option aligned with conventions
5. **Score**:
   ```
   impact_score: 1-5 (UX, Performance, Maintainability, DX)
   risk_score: 1-5 (breaking changes, affected components, test coverage)
   confidence: 0-100%
   ranking: impact × reusability / risk
   ```
6. **Write `${PROPOSAL_PATH}`** (see template below) — Use create_file tool to write the proposal
7. **Brief Summary** — Inform user in 2-3 sentences that proposal was created
8. **⚠️ STOP**

---

## PROPOSAL TEMPLATE

```markdown
# Initiative: <Title>

**Category**: Performance | Architecture | UX | TypeScript | Code Quality | etc.  
**Impact**: <1-5> | **Risk**: <1-5> | **Confidence**: <0-100%>

## Problem
<Evidence with file:line references and metrics>

## Options
1. **<Approach A>** — Complexity: S/M/L, Reusability: 1-5  
   Pros: ... | Cons: ...

2. **<Approach B>** — Complexity: S/M/L, Reusability: 1-5  
   Pros: ... | Cons: ...

## Recommendation
<Chosen option + rationale>

## Files
- Modified: <paths>
- New: <paths>

## Tests
- Unit: <what>
- Integration: <what>

---
```

---

## COMMANDS

- `@initiative` — Scan all, propose top 1 initiative, STOP
- `@initiative <category>` — Focus scan (e.g., `@initiative performance`), STOP
- `@initiative <path>` — Scan specific area, STOP

**Auto-scan priorities**:
1. High-impact quick wins (S complexity, 4-5 impact)
2. Performance bottlenecks (4-5 impact)
3. Code duplication (4-5 reusability)
4. Convention violations (90-100% confidence)

---

## EXAMPLES

**Performance**: "Optimize DataGrid: 45ms render → inline columns create new objects"  
**Architecture**: "Extract shared filter logic: 3 files × 120 lines duplicated"  
**Code Quality**: "Fix imports: React not first, violates conventions"
