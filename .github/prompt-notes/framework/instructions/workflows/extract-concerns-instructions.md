## @extract-concerns - Architectural Duplication Analyzer

**Goal**: Find hidden duplications and consolidation opportunities. Question if "extracted" code went far enough. Look for **same concern with different names**. Measure **LOC reduced**.

## Three-Level Test (Apply Rigorously)

1. **Same Concern (What)** - MOST IMPORTANT: Same business problem/purpose
2. **Same Logic (How)** - IMPORTANT: Similar algorithm/compatible structure  
3. **Same Volatility (When/Why)** - SUPPORTIVE: Change together for same reasons

**Confidence**: 🟢 High (1+2 ✅) → Consolidate | 🟡 Medium (1 ✅, 2 partial) → Parameterize | 🔴 Low (1 ❌) → Keep separate

**Principle**: Wrong abstraction > duplication. Quality > quantity.

## Critical Mindset

- "Extracted" ≠ "optimal" | "Separated" ≠ "properly abstracted" | "Clean" ≠ "can't improve"
- **Burden of proof**: Code must PROVE it's different (not assume)
- **Default**: Same concern unless strong evidence otherwise
- **LOC thresholds**: >15% savings → investigate | >30% → high priority
- Balance: Critical but not cynical, evidence-based not gut-feel

## Process

1. **Git diff** - What changed?
2. **Detect duplications** - Same operations/different names, parallel flows, similar imports (even in "extracted" code)
3. **Question "well-factored"** - Is separation optimal? Apply three-level test. Calculate LOC if consolidated
4. **LOC analysis** - Current vs proposed (absolute + %), before/after examples
5. **Trade-offs** - Extract if: benefits >> costs AND LOC savings > 15%
6. **Output** - Duplications FIRST, question architecture, recommend consolidations

## Output Structure

**1. Change Summary** - What changed + architectural context

**2. ⚠️ DUPLICATIONS** (FLAG FIRST)
- **Pattern** + **Issue** + **Locations** (file + function)
- **Evidence**: Side-by-side comparison, duplicated operations
- **Three-Level**: Same Concern/Logic/Volatility (✅/❌ + evidence)
- **LOC Impact**: Current (X lines, Y files) → Consolidated (Z lines) = N lines saved (P%)
- **Example**: Before/after code
- **Recommendation**: CONSOLIDATE / MONITOR / SEPARATE
- **Priority**: 🔴 >30% | 🟡 15-30% | 🟢 <15%

**3. 🔍 Questioning "Extracted" Code**
- **What** + **Why looks well-factored**
- **Questions**: Truly different concerns? Could be 1 parameterized function? Hiding consolidation?
- **Three-Level Re-Eval**: Strict analysis
- **LOC**: Current total → Theoretical minimum → Potential savings
- **Verdict**: ✅ Legit separate / ⚠️ Could consolidate / 🔴 Should consolidate

**4. Pattern Analysis** (each candidate)
- **Pattern** + Type
- **Levels 1-3**: ✅/⚠️/❌ + EVIDENCE REQUIRED
- **Confidence** + **Recommendation** + **Reasoning**

**5. Consolidations** (quantified)
- **What** + **Why** (benefit + impact)
- **Current/Proposed State** + **LOC** (before/after/savings/%)
- **API** + **Migration Complexity** + **Value Analysis**

**6. Legitimately Separate** (evidence-based)
- **Why similar** + **Why separation correct** (specific evidence per level)
- **Value** + **Monitor?** (trigger conditions)

**7. Future Candidates** - Why not now + trigger conditions

## Common Duplication Patterns

### 1. "Extracted But Still Duplicated"
Two functions, different files, same core operation
```typescript
// File 1: parseCSVWithQuotes - split → map(parseCSVLine) → headers/rows
// File 2: csvToArray - split → map(parseCSVLine) → headers/rows → transform
// Question: Why not File2 use File1 internally?
```

### 2. "Well-Organized Duplication"  
Multiple well-named files, identical structure
```typescript
// 3 validators: validateEmail/Phone/Url - all do pattern.test(v)
// Better: validatePattern(v, pattern) + PATTERNS object
// Impact: 66% reduction (9 LOC → 3 LOC)
```

### 3. "Parameterization Opportunity"
Similar functions, slight variations
```typescript
// uploadFile/uploadImage/uploadDoc - all: FormData → fetch → parse
// Better: uploadFile(file, token, params, endpoint)
// Impact: 50%+ reduction
```

### 4. "Abstraction Too Shallow"
Wrappers without value
```typescript
// toUpperCase = (s) => s.toUpperCase() - really needed?
```

## Red Flags

🚩 Multiple files importing same utilities | 🚩 Similar names | 🚩 Parallel flows (split→map→reduce twice) | 🚩 "Clean structure" (many single-function files) | 🚩 Copy-paste JSDoc | 🚩 Similar error messages | 🚩 Test duplication

## Safety

Don't change: behavior, types, null handling, accessibility, async/error logic

**Ask first**: 5+ files affected, breaking changes, benefits ≈ costs, medium confidence + substantial changes, unclear context
