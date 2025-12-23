# @summarize Command

## Purpose
Generate concise, brief but comprehensive summaries of prompt instruction files that preserve all relevant and necessary information.

## Usage
```
@summarize <prompt-file-path>
```

## Examples
```
@summarize ${PROMPTS_DIR}/instructions/workflows/plan-instructions.md
@summarize ${PROMPTS_DIR}/instructions/workflows/diagram-instructions.md
@summarize ${PROMPTS_DIR}/guidelines/architecture-guidelines.md
```

## Processing Instructions

When the `@summarize` command is invoked with a prompt file:

1. **Read the specified prompt file** completely
2. **Analyze the content** to identify:
   - Core objectives and goals
   - Essential rules and constraints
   - Critical workflow steps
   - Key technical requirements
   - Important patterns or examples (keep representative ones)
3. **Generate a concise version** that:
   - **Preserves all essential information** - no critical details lost
   - **Removes verbose explanations** - keep only what's necessary to understand
   - **Consolidates repetitive content** - merge similar points
   - **Uses clear, direct language** - no fluff or filler words
   - **Maintains logical structure** - keep headers and organization
   - **Keeps actionable instructions** - preserve all "must do" items
   - **Retains important examples** - only the most illustrative ones
4. **Replace the original file** with the concise version
5. **Confirm completion** with before/after line counts

## Quality Criteria

The concise version should:
- ✅ Be **50-70% shorter** than the original (target reduction)
- ✅ Retain **100% of critical information**
- ✅ Be **immediately actionable** - someone can follow it without the original
- ✅ Use **bullet points and lists** where appropriate for clarity
- ✅ Remove **introductory/concluding fluff** - get straight to the point
- ✅ Keep **technical specifications** exact (no paraphrasing that changes meaning)
- ✅ Preserve **code examples** only if they're essential to understanding

## What to Remove
- Lengthy introductions or background context
- Redundant explanations of the same concept
- Overly detailed examples when one would suffice
- Verbose transitions between sections
- Meta-commentary about the instructions themselves
- Repetitive phrasing ("it's important to...", "make sure to...")

## What to Keep
- All specific requirements and constraints
- Critical workflow steps and their order
- Technical specifications and patterns
- Essential context for understanding why
- Unique examples that illustrate key concepts
- All "must have" or "should have" items

## Output Format

Confirm completion with:
1. Original file path
2. Line count before/after
3. Percentage reduction

**Example**:
```
✅ Summarized build-instructions.md
- Before: 474 lines
- After: 78 lines
- Reduction: ~84%
```

## Notes
- The concise version **replaces the original file** - be sure content is self-contained
- Aim for **clarity over brevity** - don't sacrifice understanding for word count
- **Target 60-80% reduction** while preserving all critical information
- When in doubt, **keep it** - better to be slightly longer than to lose critical info
- Use **parallel structure** in lists for easier scanning
- **Bold** key terms for quick reference
