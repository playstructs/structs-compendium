# Entity Schemas

**Purpose**: Individual entity schema files extracted from `game-state.json` for context window efficiency.

**Use Case**: Load specific entity schemas when you need complete entity data, instead of loading the entire `game-state.json` (698 lines).

---

## Available Entity Schemas

### Core Entities

- `player.json` - Complete Player entity (~150 lines) ✅ Verified
- `planet.json` - Complete Planet entity (~100 lines) ✅ Verified
- `struct.json` - Complete Struct entity (~80 lines) ✅ Verified
- `fleet.json` - Complete Fleet entity (~80 lines) ✅ Verified
- `struct-type.json` - Complete StructType entity (~165 lines) ✅ Verified
- `guild.json` - Complete Guild entity (~80 lines) ✅ Verified

### Resource Entities

- `reactor.json` - Complete Reactor entity (~40 lines) ✅
- `substation.json` - Complete Substation entity (~40 lines) ✅

### Economic Entities

- `provider.json` - Complete Provider entity (~40 lines) ✅
- `agreement.json` - Complete Agreement entity (~50 lines) ✅
- `allocation.json` - Complete Allocation entity (~50 lines) ✅

---

## Loading Strategy

### When to Use Entity Schemas

✅ **Use entity schemas when**:
- You need complete entity data (all fields)
- Working with entity resources/attributes
- Verifying entity state for actions
- Building complex workflows involving the entity

❌ **Don't use entity schemas when**:
- Simple existence check (use `schemas/minimal/*-essential.json` instead)
- ID format verification (use `schemas/formats.json` instead)
- Basic status check (use minimal schema instead)

---

## Context Window Savings

### Before (Loading game-state.json)

**To get Player data**:
- Load: `schemas/game-state.json` (698 lines)
- Contains: All 11 entity definitions
- **Waste**: ~550 lines of unused entity definitions

### After (Loading entity schema)

**To get Player data**:
- Load: `schemas/entities/player.json` (~150 lines)
- Contains: Only Player definition
- **Savings**: 78% reduction (550 lines saved)

---

## Relationship to Other Schemas

### Minimal Schemas

Entity schemas are the "complete" version. For simple operations, use minimal schemas:
- `schemas/minimal/player-essential.json` (30 lines) - Basic info only
- `schemas/entities/player.json` (150 lines) - Complete data

### Game State Schema

The `game-state.json` file still contains all definitions for reference, but AI agents should prefer loading individual entity schemas when possible.

---

## Migration Path

### Current State

- `game-state.json` contains all entity definitions
- Entity schemas are being extracted (in progress)

### Future State

- `game-state.json` will reference entity schemas (or remain as complete reference)
- Entity schemas will be the preferred way to load entity data
- Minimal schemas will be preferred for simple operations

---

## Best Practices

1. ✅ **Start with minimal** - Use minimal schemas for simple operations
2. ✅ **Upgrade to entity** - Load entity schema when you need complete data
3. ✅ **Load only what you need** - Don't load all entity schemas at once
4. ✅ **Cache entity schemas** - They rarely change, safe to cache

---

## Verification Status

**Verified Entities** (4/11):
- ✅ `player.json` - Verified against API responses and code
- ✅ `planet.json` - Verified against code (starting properties, status)
- ✅ `struct.json` - Verified against code (status values, relationships)
- ✅ `fleet.json` - Verified against code (status values, movement requirements)

**Verified Entities** (11/11):
- ✅ `player.json` - Verified against code and database
- ✅ `planet.json` - Verified against code and database
- ✅ `struct.json` - Verified against code and database
- ✅ `fleet.json` - Verified against code and database
- ✅ `struct-type.json` - Verified against proto and database (80+ columns)
- ✅ `guild.json` - Verified against database and proto (some fields may be in gridAttributes)
- ✅ `reactor.json` - Verified against database (some fields may be in gridAttributes)
- ✅ `substation.json` - Verified against database (some fields may be in gridAttributes)
- ✅ `provider.json` - Verified against database (some fields may be in gridAttributes)
- ✅ `agreement.json` - Verified against database (some fields may be in gridAttributes)
- ✅ `allocation.json` - Verified against database (some fields may be in gridAttributes)

**Note**: These schemas represent API response structures. For code-based field definitions with formulas and calculated fields, see `schemas/entities.json#/definitions/`.

---

*Last Updated: December 7, 2025*
