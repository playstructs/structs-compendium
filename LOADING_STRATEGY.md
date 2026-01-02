# AI Documentation Loading Strategy

**Version**: 1.1.0  
**Purpose**: Guide AI agents on how to efficiently load documentation to maximize context window usage  
**Last Updated**: January 1, 2026

---

## Overview

This document provides strategies for AI agents to efficiently load Structs documentation, minimizing context window usage while maintaining access to necessary information.

**Key Principle**: **Load incrementally** - Start with minimal information, expand only as needed.

---

## Loading Tiers

### Tier 0: Minimal Load (~200 lines)

**Purpose**: Absolute minimum needed to start interacting with Structs

**Files to Load**:
1. `reference/endpoint-index.json` - Find API endpoints (~50 lines)
2. `reference/entity-index.json` - Find entity definitions (~50 lines)
3. `schemas/formats.json` - Understand ID formats (~100 lines)

**Use Case**: Initial exploration, finding what's available

**Total Size**: ~200 lines

---

### Tier 1: Essential Load (~500 lines)

**Purpose**: Basic understanding of game structure and common operations

**Files to Load** (in addition to Tier 0):
1. `schemas/minimal/player-essential.json` - Essential player info (~30 lines)
2. `schemas/minimal/planet-essential.json` - Essential planet info (~30 lines)
3. `schemas/minimal/struct-essential.json` - Essential struct info (~30 lines)
4. `protocols/query-protocol.md` - How to query (essential sections only) (~200 lines)
5. `protocols/action-protocol.md` - How to act (essential sections only) (~200 lines)

**Use Case**: Basic bot operations, simple queries

**Total Size**: ~500 lines

---

### Tier 2: Task-Specific Load (~600-1000 lines)

**Purpose**: Complete information for specific tasks

**Strategy**: Load only what's needed for the current task

**Examples**:
- **Query Task**: Load query-related schemas and protocols
- **Action Task**: Load action-related schemas and protocols
- **Economic Task**: Load economic schemas and protocols

**Use Case**: Focused bot operations, specific workflows

**Total Size**: ~600-1000 lines (varies by task)

---

### Tier 3: Complete Load (~5000+ lines)

**Purpose**: Full understanding of all systems

**Files to Load**: All schemas, protocols, patterns, examples

**Use Case**: Comprehensive bot development, full system understanding

**Total Size**: ~5000+ lines

**Note**: Rarely needed. Prefer incremental loading instead.

---

## Task-Based Loading Patterns

### Pattern 1: Query Player Information

**Goal**: Get player data

**Files to Load**:
1. `reference/endpoint-index.json` (find `/structs/player/{id}`) - ~50 lines
2. `schemas/entities/player.json` (understand structure) - ~150 lines
3. `protocols/query-protocol.md#single-entity` (query pattern) - ~50 lines
4. `schemas/responses.json#PlayerData` (response format) - ~30 lines

**Total**: ~280 lines

**Alternative (Minimal)**:
- Use `schemas/minimal/player-essential.json` (~30 lines) for simple lookups
- **Total**: ~160 lines (43% reduction)

**Loading Order**:
1. Load index → Find endpoint
2. Load entity schema → Understand structure (or minimal schema for simple operations)
3. Load protocol section → Learn query pattern
4. Load response schema → Understand response

---

### Pattern 2: Build a Struct

**Goal**: Build a structure on a planet

**Files to Load**:
1. `reference/entity-index.json` (find entities)
2. `schemas/entities/player.json` (check requirements)
3. `schemas/entities/planet.json` (check location)
4. `schemas/entities/struct.json` (understand struct)
5. `schemas/actions.json#BuildAction` (build action)
6. `api/transactions/struct-actions.yaml` (struct transactions)
7. `protocols/action-protocol.md#building` (building protocol)
8. `validation/verification.md#struct-building` (verification)

**Total**: ~800 lines

**Loading Order**:
1. Load indexes → Find relevant entities/actions
2. Load entity schemas → Understand requirements
3. Load action schema → Understand build action
4. Load protocol → Learn how to submit
5. Load validation → Learn how to verify

---

### Pattern 3: Economic Operations (Mining → Refining → Energy)

**Goal**: Complete resource extraction workflow

**Files to Load**:
1. `schemas/entities/player.json` (player resources)
2. `schemas/entities/planet.json` (planet resources)
3. `schemas/entities/struct.json` (extractor/refinery)
4. `schemas/economics.json` (economic formulas)
5. `schemas/formulas.json` (conversion rates)
6. `workflows/mine-refine-convert.json` (workflow)
7. `protocols/economic-protocol.md` (economic operations)
8. `validation/verification.md#economic` (verification)

**Total**: ~1000 lines

**Loading Order**:
1. Load workflow → Understand steps
2. Load entity schemas → Understand entities involved
3. Load economic schemas → Understand formulas
4. Load protocol → Learn operations
5. Load validation → Learn verification

---

### Pattern 4: Error Handling

**Goal**: Handle errors gracefully

**Files to Load**:
1. `schemas/errors.json` (error codes)
2. `protocols/error-handling.md#error-formats` (error formats)
3. `protocols/error-handling.md#retry-strategies` (retry patterns)
4. `examples/errors/` (error examples)

**Total**: ~600 lines

**Loading Order**:
1. Load error schema → Understand error codes
2. Load error protocol → Learn handling patterns
3. Load examples → See real error responses

---

## Incremental Loading Strategy

### Step 1: Start with Indexes

**Always load first**:
- `reference/endpoint-index.json`
- `reference/entity-index.json`
- `schemas/formats.json`

**Why**: Small files that help you find what you need without loading everything.

---

### Step 2: Load by Need

**Don't load everything at once**. Instead:

1. **Identify task** → What are you trying to do?
2. **Find relevant files** → Use indexes to find what you need
3. **Load incrementally** → Load one file at a time as needed
4. **Cache loaded files** → Don't reload if already in context

---

### Step 3: Use Section References

**When possible, load specific sections**:

Instead of:
```
Load: protocols/action-protocol.md (entire file)
```

Do:
```
Load: protocols/action-protocol.md#building (building section only)
```

**Note**: Not all tools support section loading. If not available, load full file but focus on relevant section.

---

## Caching Strategy

### What to Cache

**Cache these** (rarely change):
- Index files (`reference/*.json`)
- Format specifications (`schemas/formats.json`)
- Entity metadata (`schemas/entities/*.json`)

**Don't cache these** (may change):
- Game state queries (always fresh)
- Error responses (context-dependent)
- Examples (reference only)

---

### Cache Invalidation

**Invalidate cache when**:
- Documentation version changes
- Schema structure changes
- API endpoints change

**Check version fields** in schemas to detect changes.

---

## File Size Reference

### Small Files (< 100 lines) - Load Freely
- `schemas/formats.json` (~100 lines)
- `schemas/minimal/*.json` (~30-50 lines each)
- `reference/*.json` (~50-100 lines each)
- `config/*.json` (~50 lines each)

### Medium Files (100-300 lines) - Load as Needed
- `schemas/entities/*.json` (~150-200 lines each)
- `schemas/actions.json` (~300 lines)
- `protocols/*.md` (varies, load sections when possible)
- `tasks/*.json` (~100-200 lines each)
- `workflows/*.json` (~100-200 lines each)

### Large Files (300+ lines) - Load Sections Only
- `schemas/game-state.json` (699 lines) - **Load entity-specific sections**
- `api/endpoints.yaml` (1153 lines) - **Load endpoint-specific sections**
- `protocols/error-handling.md` (650 lines) - **Load error-specific sections**

---

## Optimization Tips

### Tip 1: Use Indexes First

**Before loading large files**, check indexes:
- `reference/endpoint-index.json` → Find exact endpoint
- `reference/entity-index.json` → Find exact entity
- Then load only what you need

**Savings**: Avoid loading 1000+ line files when you only need 50 lines.

---

### Tip 2: Load Minimal Schemas for Lookups

**For simple lookups**, use minimal schemas:
- `schemas/minimal/player-essential.json` (30 lines)
- Instead of `schemas/entities/player.json` (200 lines)

**Savings**: 85% reduction for simple operations.

---

### Tip 3: Load Workflows for Multi-Step Operations

**For complex operations**, load workflow files:
- `workflows/mine-refine-convert.json` (complete workflow)
- Instead of loading all individual schemas separately

**Savings**: Single file with all steps vs. multiple files.

---

### Tip 4: Use Decision Trees for Logic

**For decision-making**, load decision trees:
- `patterns/decision-tree-resource-security.json`
- Instead of reading through multiple protocol files

**Savings**: Structured logic in one file vs. scattered documentation.

---

### Tip 5: Load Examples for Patterns

**For understanding patterns**, load examples:
- `examples/gameplay-mining-bot.json` (complete example)
- Instead of piecing together from multiple sources

**Savings**: Concrete example vs. abstract documentation.

---

## Common Mistakes to Avoid

### ❌ Mistake 1: Loading Everything at Once

**Don't do this**:
```
Load: All schemas, all protocols, all examples (5000+ lines)
```

**Do this instead**:
```
1. Load indexes
2. Identify what you need
3. Load incrementally
```

---

### ❌ Mistake 2: Loading Full Files for Simple Queries

**Don't do this**:
```
Load: schemas/game-state.json (699 lines) to check player ID format
```

**Do this instead**:
```
Load: schemas/formats.json (100 lines) for ID format
```

---

### ❌ Mistake 3: Not Using Indexes

**Don't do this**:
```
Load: api/endpoints.yaml (1153 lines) to find one endpoint
```

**Do this instead**:
```
1. Load: reference/endpoint-index.json (50 lines)
2. Find endpoint
3. Load: api/queries/player.yaml (100 lines) if needed
```

---

### ❌ Mistake 4: Reloading Cached Information

**Don't do this**:
```
Load: schemas/formats.json (already loaded 5 times)
```

**Do this instead**:
```
Cache: schemas/formats.json (load once, reuse)
```

---

## Loading Examples

### Example 1: Simple Bot - Query Player

```json
{
  "loadingStrategy": "minimal-incremental",
  "files": [
    {
      "file": "reference/endpoint-index.json",
      "purpose": "Find player endpoint",
      "size": "~50 lines"
    },
    {
      "file": "schemas/minimal/player-essential.json",
      "purpose": "Understand player structure",
      "size": "~30 lines"
    },
    {
      "file": "protocols/query-protocol.md#single-entity",
      "purpose": "Learn query pattern",
      "size": "~50 lines"
    }
  ],
  "totalSize": "~130 lines",
  "efficiency": "Excellent - only what's needed"
}
```

---

### Example 2: Complex Bot - Build Struct Workflow

```json
{
  "loadingStrategy": "task-based-incremental",
  "files": [
    {
      "file": "reference/entity-index.json",
      "purpose": "Find entities",
      "size": "~50 lines"
    },
    {
      "file": "schemas/entities/player.json",
      "purpose": "Check player requirements",
      "size": "~200 lines"
    },
    {
      "file": "schemas/entities/planet.json",
      "purpose": "Check planet location",
      "size": "~200 lines"
    },
    {
      "file": "schemas/actions.json#BuildAction",
      "purpose": "Understand build action",
      "size": "~100 lines"
    },
    {
      "file": "protocols/action-protocol.md#building",
      "purpose": "Learn building protocol",
      "size": "~150 lines"
    },
    {
      "file": "validation/verification.md#struct-building",
      "purpose": "Learn verification",
      "size": "~100 lines"
    }
  ],
  "totalSize": "~800 lines",
  "efficiency": "Good - focused on task"
}
```

---

## Version Compatibility

### Checking Versions

**All schemas include version fields**:
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "version": "1.0.0",
  ...
}
```

**Check versions** to ensure compatibility:
- Major version changes = Breaking changes
- Minor version changes = New features (backward compatible)
- Patch version changes = Bug fixes (backward compatible)

---

## Best Practices Summary

1. ✅ **Start with indexes** - Find what you need first
2. ✅ **Load incrementally** - One file at a time as needed
3. ✅ **Use minimal schemas** - For simple operations
4. ✅ **Load workflows** - For multi-step operations
5. ✅ **Cache static files** - Don't reload indexes/formats
6. ✅ **Use section references** - Load specific sections when possible
7. ✅ **Check file sizes** - Prefer smaller files when possible
8. ✅ **Follow task patterns** - Use established loading patterns

---

## Quick Reference

### Minimal Load (Start Here)
```
reference/endpoint-index.json
reference/entity-index.json
schemas/formats.json
```

### Common Tasks
- **Query**: endpoint-index → entity schema → query protocol
- **Action**: entity-index → action schema → action protocol → validation
- **Error**: error schema → error protocol → error examples
- **Workflow**: workflow file → related schemas → related protocols

### File Size Guide
- **Small** (< 100 lines): Load freely
- **Medium** (100-300 lines): Load as needed
- **Large** (300+ lines): Load sections only

---

*Last Updated: January 1, 2026*

