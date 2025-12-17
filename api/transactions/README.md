# Transaction Endpoints

**Version**: 1.0.0  
**Purpose**: Transaction endpoints split from `endpoints.yaml` for context window efficiency

---

## Overview

This directory contains transaction endpoints. All game actions are submitted through the transaction endpoint.

**Use Case**: Load transaction endpoint documentation when submitting actions, instead of loading the entire `endpoints.yaml` (1153 lines).

---

## Available Files

- **`submit-transaction.yaml`** - Submit transaction endpoint (~60 lines)

---

## Context Window Savings

### Before (Loading endpoints.yaml)

**To get transaction endpoint**:
- Load: `api/endpoints.yaml` (1153 lines)
- Contains: All query, transaction, and webapp endpoints
- **Waste**: ~1090 lines of unused endpoints

### After (Loading transaction file)

**To get transaction endpoint**:
- Load: `api/transactions/submit-transaction.yaml` (~60 lines)
- Contains: Only transaction endpoint
- **Savings**: 95% reduction (1090 lines saved)

---

## Usage

### Loading Transaction Endpoint

```json
{
  "load": "api/transactions/submit-transaction.yaml"
}
```

---

## Related Documentation

- **Main Endpoints**: `../endpoints.yaml` - Complete endpoint catalog (index)
- **Queries**: `../queries/` - Query endpoints
- **Actions**: `../../schemas/actions.json` - Action message types
- **Protocols**: `../../protocols/action-protocol.md` - How to perform actions

---

*Last Updated: January 2025*
