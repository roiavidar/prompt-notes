# @generate-command Command

## Objective
Generate a project-specific command file from a specification template by filling in context details while preserving the original essence, tone, structure, and emphasis.

**Purpose**: Convert portable spec templates into concrete, project-ready instruction files.

---

## Input Modes
```
@generate-command <spec-file-path>
@generate-command <spec-filename>
```

**Examples**:
```
@generate-command ${PROMPTS_DIR}/util/build-instructions-spec.md
@generate-command test-instructions-spec.md
@generate-command plan-instructions-spec
```

---

## Workflow

### Phase 1: Read Spec File
1. **Read complete spec file** - All three sections (Essence, Template, Context Questions)
2. **Extract all {{VARIABLES}}** - Build complete list with descriptions
3. **Understand essence** - Capture the universal methodology
4. **Review template structure** - Note markdown hierarchy, emphasis, examples

### Phase 2: Gather Context (Answer Questions)

**For each Context Question**:

1. **Read the question** and understand what information is needed
2. **Search the codebase** to discover the answer:
   - Use `semantic_search` for concepts/patterns
   - Use `grep_search` for specific strings/commands
   - Use `file_search` for file/directory patterns
   - Use `read_file` to examine relevant files
   - Use `list_dir` to explore directory structures

3. **Validate the answer** using the validation method from the question

4. **Document findings**:
   ```markdown
   ## {{VARIABLE_NAME}}
   **Value**: <discovered-value>
   **Source**: <where-found>
   **Validation**: <how-verified>
   ```

**Question Answering Strategy**:
- **Paths**: Search for directory names, list_dir to verify structure
- **Commands**: Check package.json scripts, search for npm/yarn usage patterns
- **Technologies**: Read package.json dependencies, search imports
- **Patterns**: Semantic search for similar code, read examples
- **Conventions**: Read style guides, analyze existing code samples

**CRITICAL**: 
- ✅ Use ACTUAL project values - never guess or use examples
- ✅ Verify each value exists/works in the project
- ✅ Cross-check dependencies between variables
- ❌ Do NOT use placeholder values in final command
- ❌ Do NOT skip validation steps

### Phase 3: Fill Template

**Replace placeholders with discovered values**:
1. For each `{{VARIABLE}}` in template
2. Substitute with actual project value
3. Preserve ALL surrounding text exactly
4. Keep emphasis words (CRITICAL, MUST, etc.)
5. Maintain markdown structure
6. Keep examples with real values

**Transformation Rules**:
```markdown
BEFORE: "Run `{{TYPECHECK_CMD}}` to check types"
AFTER: "Run `yarn tsc --noEmit` to check types"

BEFORE: "Read `{{GUIDELINES_PATH}}/code-style-guidelines.md`"
AFTER: "Read `${PROMPTS_DIR}/guidelines/code-style-guidelines.md`"

BEFORE: "Search `{{COMMON_DIR}}/` for patterns"
AFTER: "Search `src/common/` for patterns"
```

**Preservation Requirements**:
- ✅ Keep ALL original text word-for-word
- ✅ Preserve emphasis (CRITICAL, MUST, MUST NOT, ALWAYS, NEVER)
- ✅ Maintain structure (headers, lists, indentation, code blocks)
- ✅ Keep tone (formal/casual) exactly as specified
- ✅ Preserve generalization level (don't make more/less specific)
- ✅ Retain all examples (with real values substituted)
- ❌ Do NOT add explanations not in spec
- ❌ Do NOT remove content unless necessary
- ❌ Do NOT change wording beyond variable substitution

### Phase 4: Necessity Check (Modifications)

**BEFORE modifying the output command file** (adding/removing/changing content beyond variable substitution):

**CRITICAL**: This is about modifying the **generated command file output**, NOT the spec file itself. The spec file remains unchanged.

**When context answers demand changes**:
- If discovered project values require structural changes to work
- If project-specific constraints make spec template incompatible
- If context reveals the spec approach won't work for this project

**Ask yourself**:
1. Is this modification absolutely necessary?
2. Does the original template not work without it?
3. Is there a project-specific reason requiring this change?
4. Did the context discovery reveal incompatibility?

**If YES to all**:
1. Make the minimal change needed
2. Document in `## Generation Notes` section at end of **generated command file**:
   ```markdown
   ## Generation Notes
   
   **Modifications Made**:
   1. **Added**: [What] at [Location]
      - **Reason**: [Why it was necessary]
      - **Justification**: [Project-specific requirement]
   
   2. **Removed**: [What] from [Location]
      - **Reason**: [Why it couldn't stay]
      - **Justification**: [Incompatibility or constraint]
   
   3. **Changed**: [What] at [Location]
      - **Original**: [Old text]
      - **New**: [New text]
      - **Reason**: [Why change was necessary]
      - **Justification**: [Project-specific requirement]
   ```

**If NO**:
- Keep spec template exactly as is (with variables filled)
- The generated command file matches spec template structure 100%

**Examples of justified modifications**:
- Spec assumes REST API, project uses GraphQL → add GraphQL-specific steps
- Spec references npm, project uses yarn → adjust command examples
- Spec assumes monorepo structure, project is single-repo → simplify paths
- Context reveals missing validation step critical for this project → add it

**Examples of unjustified modifications**:
- "This wording sounds better" → NO, preserve original
- "Let me add more examples" → NO, keep spec examples only
- "I'll make it more detailed" → NO, match spec generalization level
- "Let me reorganize sections" → NO, preserve spec structure

### Phase 5: Generate Command File

**Create** command file in appropriate location:
- Workflow commands: `{{PROMPTS_DIR}}/instructions/workflows/<command-name>-instructions.md`
- Utility commands: `{{PROMPTS_DIR}}/util/<command-name>-instructions.md`
- Guidelines: `{{PROMPTS_DIR}}/guidelines/<guideline-name>-guidelines.md`

**File Structure**:
```markdown
# @<command-name> Command
[or]
# <Guideline Name> Guidelines

[Template content with ALL {{VARIABLES}} replaced by actual values]

[Original sections, structure, content preserved exactly]

[Examples now contain real project values]

---

## Generation Notes

**Generated from**: `<spec-file-path>`  
**Date**: <timestamp>  
**Project**: <project-name>

**Variables Filled**:
- {{VAR1}}: <value> - <source>
- {{VAR2}}: <value> - <source>
- {{VAR3}}: <value> - <source>

**Modifications**: [None] or [See below]

[If modifications made, document them as per Phase 4]
```

### Phase 6: Validation

**Verify command file quality**:
- [ ] ALL {{VARIABLES}} replaced with actual values
- [ ] All values validated against project
- [ ] Original tone preserved (no softening/strengthening)
- [ ] Original structure maintained (headers, lists, code blocks)
- [ ] Emphasis words kept exactly (CRITICAL, MUST, etc.)
- [ ] Examples contain real project values
- [ ] Generalization level matches spec
- [ ] No unnecessary additions/removals
- [ ] Modifications (if any) documented with justification
- [ ] File is immediately actionable
- [ ] Cross-references (file paths, commands) are valid

**Test command references**:
- [ ] File paths exist in project
- [ ] Commands work (check package.json scripts)
- [ ] Referenced files/dirs are accessible
- [ ] Examples use real, working values

### Phase 7: Register Command (If Workflow/Util)

**If creating new command**, add to commands summary:

1. **Read** existing commands file (if exists)
2. **Add entry** for new command:
   ```markdown
   **`@<command-name>`** - <one-line-description>
   - Example: `"@<command> <usage-example>"`
   ```
3. **Update** in appropriate section (workflows/utilities)

### Phase 8: Report

**Output**:
```markdown
✅ Command generated: `<command-file-path>`

**Context Discovery Summary**:
- Variables filled: <count>
- Sources searched: <count>
- Values validated: <count>
- Modifications: <count> [or "None"]

**Variables Applied**:
- {{VAR1}}: <value>
- {{VAR2}}: <value>
- {{VAR3}}: <value>

**Validation**:
- ✅ All paths verified
- ✅ All commands tested
- ✅ Structure preserved
- ✅ Tone matched
- ✅ Examples use real values

**Modifications** (if any):
[List changes with justifications]

**Next Steps**:
1. Review generated command at `<file-path>`
2. Test command in your workflow:
   ```
   @<command-name> <example-usage>
   ```
3. Adjust project.config.md if needed
```

---

## Context Discovery Strategies

**Finding Paths**:
```javascript
// Search for directory names
semantic_search("common components directory")
file_search("**/common/components/**")
list_dir("src/")

// Verify structure
list_dir("src/common/components/")
```

**Finding Commands**:
```javascript
// Check package.json
read_file("package.json")
grep_search("\"scripts\":", false, "package.json")

// Search for command usage
grep_search("yarn tsc", false)
grep_search("npm run", false)
```

**Finding Technologies**:
```javascript
// Check dependencies
read_file("package.json")
semantic_search("graphql apollo setup")
grep_search("import.*from.*@mui", true)
```

**Finding Patterns**:
```javascript
// Search for existing implementations
semantic_search("error handling pattern")
semantic_search("form validation example")
grep_search("makeStyles", true)
```

**Finding Conventions**:
```javascript
// Read style guides
file_search("**/guidelines/**")
read_file("guidelines/code-style-guidelines.md")

// Analyze existing code
semantic_search("import ordering convention")
grep_search("import React from", true)
```

---

## Key Principles

🔍 **Discover, Don't Assume** - Search codebase for actual values  
✅ **Validate Everything** - Verify paths, commands, patterns exist  
🎯 **Preserve Essence** - Keep tone, structure, emphasis exactly  
📝 **Document Changes** - Justify ANY modifications made  
🚫 **Minimal Modifications** - Only change when absolutely necessary  
🔄 **Cross-Reference** - Ensure variable dependencies are consistent  

---

## Quality Checklist

- [ ] ALL {{VARIABLES}} discovered and validated
- [ ] Context gathered from actual project code
- [ ] Original tone/emphasis/structure preserved
- [ ] Examples use real project values
- [ ] File paths exist and are accessible
- [ ] Commands work (verified in package.json)
- [ ] No placeholders remain in output
- [ ] Modifications (if any) are justified
- [ ] Command is immediately actionable
- [ ] Generation notes document process

---

## Error Recovery

**Missing variable value**: 
- Search harder (try alternative queries)
- Check related files (config files, docs)
- Ask user if truly unable to discover

**Invalid value**:
- Re-validate using different method
- Check typos or outdated paths
- Update project.config.md if needed

**Conflicting patterns**:
- Document both patterns in notes
- Choose most common/recent pattern
- Explain decision in generation notes

**Modification needed**:
- MUST justify in Generation Notes
- Explain why spec template wouldn't work for this project
- Document exact change and reason
- Reference context answers that revealed incompatibility
- Keep changes minimal and necessary only

---

## Notes

- **Spec file is never modified** - Only the generated command file output changes
- **Actual values only** - Never leave placeholders in final command
- **Validation is mandatory** - Don't skip verification steps
- **Modifications need justification** - Document WHY in Generation Notes when output differs from spec
- **Context-driven changes** - Only modify output when context answers reveal incompatibility
- **Tone preservation is CRITICAL** - Don't change emphasis or wording unless absolutely necessary
- **Structure is sacred** - Keep markdown hierarchy identical unless project demands otherwise
- **Examples are important** - Replace placeholders but keep structure
- **Cross-reference variables** - Ensure paths/commands are consistent
- **Test the output** - Generated command should work immediately
