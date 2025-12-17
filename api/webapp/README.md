# Webapp API Endpoints

**Version**: 1.0.0  
**Purpose**: Webapp endpoints split from `endpoints.yaml` for context window efficiency

---

## Overview

This directory contains webapp API endpoints organized by entity type. This allows AI agents to load only the webapp endpoints they need, reducing context window usage.

**Implementation**: The webapp API is implemented in a PHP Symfony application (`structs-webapp`). This is the main user-facing API for the game. API authentication is implemented in the PHP Symfony application.

**Use Case**: Load specific entity webapp endpoints when working with that entity, instead of loading the entire `endpoints.yaml` (1153 lines).

---

## Available Files

### Entity-Specific Webapp Endpoints

- **`player.yaml`** - Player webapp endpoints (8 endpoints, ~130 lines)
- **`planet.yaml`** - Planet webapp endpoints (5 endpoints, ~80 lines)
- **`guild.yaml`** - Guild webapp endpoints (11 endpoints, ~150 lines)
- **`auth.yaml`** - Authentication endpoints (3 endpoints, ~30 lines)
- **`struct.yaml`** - Struct webapp endpoints (3 endpoints, ~40 lines)
- **`ledger.yaml`** - Ledger endpoints (3 endpoints, ~50 lines)
- **`infusion.yaml`** - Infusion endpoints (1 endpoint, ~20 lines)
- **`system.yaml`** - System endpoints (timestamp, 1 endpoint, ~30 lines)

**Total**: 34 webapp endpoints across 8 entity files

---

## Context Window Savings

### Before (Loading endpoints.yaml)

**To get Player webapp endpoints**:
- Load: `api/endpoints.yaml` (1153 lines)
- Contains: All query, transaction, and webapp endpoints
- **Waste**: ~1050 lines of unused endpoints

### After (Loading entity webapp file)

**To get Player webapp endpoints**:
- Load: `api/webapp/player.yaml` (~100 lines)
- Contains: Only Player webapp endpoints
- **Savings**: 91% reduction (1050 lines saved)

---

## Usage

### Loading Entity Webapp Endpoints

```json
{
  "load": "api/webapp/player.yaml"
}
```

### Loading Multiple Entities

```json
{
  "load": [
    "api/webapp/player.yaml",
    "api/webapp/planet.yaml"
  ]
}
```

---

## Related Documentation

- **Main Endpoints**: `../endpoints.yaml` - Complete endpoint catalog (index)
- **Queries**: `../queries/` - Query endpoints
- **Transactions**: `../transactions/` - Transaction endpoints
- **Protocol**: `../../protocols/webapp-api-protocol.md` - Webapp API usage guide
- **Loading Strategy**: `../../LOADING_STRATEGY.md` - How to load efficiently

---

*Last Updated: December 7, 2025*

