# @generate-spec Command

## Objective
Extract the essence of an instruction file into a portable, reusable template that preserves tone, structure, emphasis, and generalization level while separating project-specific details.

**Purpose**: Create specification templates that can be reused across different projects by filling in context-specific details.

---

## Input Mode
```
@generate-spec <instruction-file-path>
@generate-spec <instruction-filename>
```

**Examples**:
```
@generate-spec ${PROMPTS_DIR}/instructions/workflows/build-instructions.md
@generate-spec build-instructions.md
@generate-spec test-instructions
```

---

## Workflow

### Phase 1: Read Source Instruction File
1. **Read the entire instruction file** - No truncation, get complete context
2. **Identify file structure**:
   - Section headers (##, ###, etc.)
   - Workflow phases/steps
   - Rules and constraints
   - Examples and patterns
   - Code blocks and templates
   - Emphasis words (CRITICAL, MUST, MUST NOT, ALWAYS, NEVER, etc.)

### Phase 2: Extract Essence
**Analyze each section** and classify content:

**Universal Elements** (keep verbatim):
- Workflow methodology and structure
- Phase names and ordering
- Rule types and categories
- Emphasis levels (CRITICAL, MUST, etc.)
- Logical flow and dependencies
- Best practices and principles
- Error handling patterns
- Quality criteria

**Project-Specific Elements** (mark for context):
- File paths and directory structures
- Command names and invocations
- Specific technology names
- Project-specific patterns
- Tool-specific syntax
- Framework-specific details
- Environment variables

**Key Principle**: Extract the **"what"** and **"how"** (universal), mark the **"where"** and **"which"** (project-specific).

### Phase 3: Generate 3-Section Output

Create new file: `<original-name>-spec.md` in same directory

**Section 1: Essence**
```markdown
## Essence

[Pure methodology - no project specifics]
- What the command does (abstract goal)
- Why it exists (problem it solves)
- Core workflow (universal steps)
- Key principles (timeless rules)
- Quality criteria (universal standards)

**Preserve**:
- Original tone (formal/casual)
- Emphasis words (CRITICAL, MUST, etc.)
- Structure (phases, sections, hierarchy)
- Generalization level (specific vs abstract)
```

**Section 2: Template**
```markdown
## Template

[Instruction structure with placeholders]

Replace project-specific content with:
- `{{VARIABLE_NAME}}` for paths, commands, files
- `{{TECHNOLOGY}}` for frameworks, tools, libraries
- `{{PATTERN}}` for project-specific patterns
- Keep all original text that isn't project-specific
- Preserve markdown structure exactly
- Keep all emphasis words (CRITICAL, MUST, etc.)
- Maintain original examples with placeholders

**Example Transformation**:
BEFORE: "Run `yarn tsc --noEmit` to check types"
AFTER: "Run `{{TYPECHECK_CMD}}` to check types"

BEFORE: "Read `${PROMPTS_DIR}/guidelines/code-style-guidelines.md`"
AFTER: "Read `{{GUIDELINES_PATH}}/code-style-guidelines.md`"

**CRITICAL**: 
- Do NOT change wording beyond placeholder substitution
- Do NOT add explanations not in original
- Do NOT remove examples or rules
- Do NOT change tone or emphasis level
```

**Section 3: Context Questions**
```markdown
## Context Questions

[Comprehensive questionnaire to fill template]

For each `{{VARIABLE}}` in template, provide:
1. **Variable Name**: `{{VARIABLE_NAME}}`
2. **Description**: What this represents
3. **Question**: How to determine the correct value?
4. **Examples**: 2-3 realistic values
5. **Validation**: How to verify the value is correct?
6. **Dependencies**: What other variables does this relate to?

**Question Categories**:
- **Paths**: Where are files/directories located?
- **Commands**: What commands does the project use?
- **Technologies**: What stack/frameworks are used?
- **Patterns**: What code patterns exist in the codebase?
- **Conventions**: What naming/styling rules apply?

**Question Quality Criteria**:
- ✅ Specific enough to guide discovery
- ✅ Includes validation methods (how to verify answer)
- ✅ Provides concrete examples
- ✅ Explains why the detail matters
- ✅ Cross-references related questions
```

### Phase 4: Generate Spec File

**Create** `<instruction-name>-spec.md`:
```markdown
# {{COMMAND_NAME}} Specification

**Generated from**: `<original-file-path>`  
**Date**: <timestamp>

---

## Essence

[Universal methodology extracted from original]

---

## Template

[Instruction with placeholders]

---

## Context Questions

[Comprehensive questionnaire]

---

## Generation Notes

**Variables Identified**: [List all {{VARIABLES}}]
**Emphasis Words Preserved**: [List CRITICAL, MUST, etc. counts]
**Structure Preserved**: [Section count, hierarchy depth]
**Examples Transformed**: [Count]
```

### Phase 5: Validation

**Check spec file quality**:
- [ ] All project-specific details replaced with `{{VARIABLES}}`
- [ ] Original tone and emphasis preserved exactly
- [ ] Structure (headers, lists, code blocks) unchanged
- [ ] Generalization level matches original
- [ ] No new rules or explanations added
- [ ] All emphasis words (CRITICAL, MUST, etc.) kept
- [ ] Examples converted to templates with placeholders
- [ ] Context questions cover ALL variables comprehensively
- [ ] Questions include validation methods
- [ ] Questions provide concrete examples

### Phase 6: Report

**Output**:
```markdown
✅ Spec generated: `<spec-file-path>`

**Transformation Summary**:
- Original: <line-count> lines
- Essence: <line-count> lines
- Template: <line-count> lines  
- Context Questions: <question-count> questions
- Variables Extracted: <variable-count>

**Variables**: {{VAR1}}, {{VAR2}}, {{VAR3}}...

**Next Steps**:
1. Review spec file for accuracy
2. Use `@generate-command` to create project-specific command:
   ```
   @generate-command <spec-file-path>
   ```
```

---

## Key Principles

🎯 **Preserve Essence** - Capture methodology, not implementation  
🔄 **Exact Tone** - Keep emphasis, structure, generalization level  
📝 **Mark Specifics** - Replace project details with {{VARIABLES}}  
❓ **Comprehensive Questions** - Guide complete template filling  
✅ **Validate Quality** - Check preservation and completeness  
🚫 **No Additions** - Don't add rules/explanations not in original

---

## Quality Checklist

- [ ] Essence captures universal methodology
- [ ] Template has placeholders for ALL project-specific details
- [ ] Original tone/emphasis/structure preserved exactly
- [ ] Context questions are comprehensive and actionable
- [ ] Questions include validation methods
- [ ] Examples converted to templates
- [ ] No content added or removed unnecessarily
- [ ] Spec file is self-contained and complete

---

## Error Recovery

**Missing context**: Read entire file, don't truncate
**Unclear boundaries**: When in doubt, mark as {{VARIABLE}}
**Complex patterns**: Create multiple related questions
**Validation fails**: Document what was changed and why (MUST justify)

---

## Notes

- **Tone preservation is CRITICAL** - Don't soften MUST to should, don't strengthen may to MUST
- **Structure is sacred** - Keep markdown hierarchy, lists, code blocks identical
- **Examples are templates** - Convert to placeholders but keep structure
- **Questions must be actionable** - Include "how to find this" guidance
- **Validation is required** - Each question needs verification method
