# Query Endpoints

**Version**: 1.0.0  
**Purpose**: Query endpoints split from `endpoints.yaml` for context window efficiency

---

## Overview

This directory contains query endpoints organized by entity type. This allows AI agents to load only the endpoints they need, reducing context window usage.

**Use Case**: Load specific entity query endpoints when working with that entity, instead of loading the entire `endpoints.yaml` (1153 lines).

---

## Available Query Files

### Core Entity Queries

- **`player.yaml`** - Player query endpoints (~50 lines)
- **`planet.yaml`** - Planet query endpoints (~60 lines)
- **`struct.yaml`** - Struct query endpoints (~40 lines)
- **`fleet.yaml`** - Fleet query endpoints (~50 lines)
- **`guild.yaml`** - Guild query endpoints (~40 lines)

### Resource Entity Queries

- **`reactor.yaml`** - Reactor query endpoints (~40 lines) ✅
- **`substation.yaml`** - Substation query endpoints (~40 lines) ✅

### Economic Entity Queries

- **`provider.yaml`** - Provider query endpoints (~40 lines) ✅
- **`agreement.yaml`** - Agreement query endpoints (~60 lines) ✅
- **`allocation.yaml`** - Allocation query endpoints (~70 lines) ✅

### System Queries

- **`system.yaml`** - System queries (block-height, params, etc.) (~50 lines) ✅

### Other Queries

- **`address.yaml`** - Address query endpoints (~50 lines) ✅
- **`permission.yaml`** - Permission query endpoints (~70 lines) ✅

---

## Context Window Savings

### Before (Loading endpoints.yaml)

**To get Player endpoints**:
- Load: `api/endpoints.yaml` (1153 lines)
- Contains: All query, transaction, and webapp endpoints
- **Waste**: ~1100 lines of unused endpoints

### After (Loading entity query file)

**To get Player endpoints**:
- Load: `api/queries/player.yaml` (~50 lines)
- Contains: Only Player query endpoints
- **Savings**: 96% reduction (1100 lines saved)

---

## Usage

### Loading Entity Queries

```json
{
  "load": "api/queries/player.yaml"
}
```

### Loading Multiple Entities

```json
{
  "load": [
    "api/queries/player.yaml",
    "api/queries/planet.yaml"
  ]
}
```

---

## Related Documentation

- **Main Endpoints**: `../endpoints.yaml` - Complete endpoint catalog (index)
- **Transactions**: `../transactions/` - Transaction endpoints
- **Webapp**: `../webapp/` - Webapp API endpoints (if split)
- **Loading Strategy**: `../../LOADING_STRATEGY.md` - How to load efficiently

---

*Last Updated: January 2025*
