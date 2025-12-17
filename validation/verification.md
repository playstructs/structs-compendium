# Verification

**Version**: 1.0.0  
**Category**: Validation  
**Status**: Stable

## Overview

This document describes how to verify that actions actually occurred after transaction broadcast. **Always verify game state** - a transaction showing "broadcast" doesn't mean the action succeeded!

---

## Why Verification is Critical

### The Problem

**Transaction status "broadcast" ≠ Action succeeded**

Validation happens on-chain AFTER broadcast. If requirements aren't met, the transaction will broadcast but the action won't happen.

### The Solution

**Always verify game state after broadcast** to confirm actions actually occurred.

---

## Verification Pattern

### Basic Pattern

```json
{
  "verificationPattern": {
    "1": "Submit transaction",
    "2": "Wait for transaction to reach 'broadcast' status",
    "3": "Query game state to verify action actually occurred",
    "4": "If action didn't occur, check requirements",
    "5": "Fix requirements and retry if needed"
  }
}
```

### Detailed Pattern

```json
{
  "detailedPattern": {
    "step1": "Submit transaction",
    "step2": "Wait for 'broadcast' status",
    "step3": {
      "action": "Query game state",
      "endpoint": "Relevant entity endpoint",
      "filter": "By expected identifiers",
      "expected": "Entity exists or state changed"
    },
    "step4": {
      "compare": "Expected vs actual",
      "ifMatch": "Action succeeded ✅",
      "ifMismatch": "Action failed - check requirements"
    },
    "step5": {
      "ifFailed": "Identify missing requirement",
      "fix": "Fix requirement",
      "retry": "Retry action",
      "verify": "Verify again"
    }
  }
}
```

---

## Verification Examples

### Example 1: Building Struct

**Action**: Build struct on planet

**Verification**:
```json
{
  "verification": {
    "afterBroadcast": {
      "check": "Query struct entity",
      "endpoint": "/structs/struct",
      "filter": "By location_id = planetId AND struct_type = structType",
      "expected": "Struct entity exists",
      "actual": "Query result",
      "match": "Compare expected vs actual"
    }
  }
}
```

**Success Criteria**:
- Struct entity exists
- Struct location_id = planetId
- Struct struct_type = expected type
- Struct status = materialized (1) or built

**Failure Indicators**:
- No struct found
- Struct location_id different
- Struct struct_type different

---

### Example 2: Exploration

**Action**: Explore new planet

**Verification**:
```json
{
  "verification": {
    "afterBroadcast": {
      "checks": [
        {
          "check": "Player owns new planet",
          "endpoint": "/structs/player/{playerId}",
          "expected": "player.planet_id = newPlanetId",
          "actual": "Query player entity"
        },
        {
          "check": "New planet exists",
          "endpoint": "/structs/planet/{newPlanetId}",
          "expected": "Planet entity exists",
          "actual": "Query planet entity"
        },
        {
          "check": "Fleet moved to new planet",
          "endpoint": "/structs/fleet/{fleetId}",
          "expected": "fleet.planet_id = newPlanetId",
          "actual": "Query fleet entity"
        }
      ]
    }
  }
}
```

**Success Criteria**:
- Player planet_id changed to new planet
- New planet entity exists
- Fleet planet_id = new planet
- Old planet exists but player no longer owns it

**Failure Indicators**:
- Player planet_id unchanged
- No new planet created
- Fleet didn't move

---

### Example 3: Command Ship Activation

**Action**: Activate Command Ship

**Verification**:
```json
{
  "verification": {
    "afterBroadcast": {
      "check": "Query Command Ship struct",
      "endpoint": "/structs/struct/{commandShipId}",
      "expected": "Struct status = online",
      "actual": "Query struct entity",
      "additional": "Check player power consumption increased"
    }
  }
}
```

**Success Criteria**:
- Command Ship status = online
- Player power consumption increased
- Command Ship consuming power

**Failure Indicators**:
- Command Ship status still = built (not online)
- No power consumption change

---

## Verification Checklist

### Pre-Action Verification

Before submitting transaction:

```json
{
  "preActionChecks": {
    "playerOnline": "Query player status - must be online",
    "requirements": "Check all action requirements",
    "gameState": "Query current game state",
    "expectedResult": "Define expected result"
  }
}
```

### Post-Action Verification

After transaction broadcast:

```json
{
  "postActionChecks": {
    "transactionStatus": "Verify transaction reached 'broadcast'",
    "gameState": "Query game state",
    "compare": "Compare expected vs actual",
    "ifMismatch": "Check requirements and retry"
  }
}
```

---

## Common Verification Patterns

### Pattern 1: Entity Creation

**Use Case**: Building structs, creating entities

**Pattern**:
```json
{
  "pattern": {
    "action": "Create entity",
    "verification": {
      "endpoint": "/structs/{entityType}",
      "filter": "By expected identifiers",
      "expected": "Entity exists",
      "check": "Query entity, verify exists"
    }
  }
}
```

### Pattern 2: State Change

**Use Case**: Activating structs, changing status

**Pattern**:
```json
{
  "pattern": {
    "action": "Change entity state",
    "verification": {
      "endpoint": "/structs/{entityType}/{entityId}",
      "expected": "State changed",
      "check": "Query entity, verify state"
    }
  }
}
```

### Pattern 3: Relationship Change

**Use Case**: Exploration, ownership changes

**Pattern**:
```json
{
  "pattern": {
    "action": "Change relationship",
    "verification": {
      "endpoints": [
        "/structs/{entityType1}/{id1}",
        "/structs/{entityType2}/{id2}"
      ],
      "expected": "Relationship changed",
      "check": "Query both entities, verify relationship"
    }
  }
}
```

---

## Error Detection

### Detecting Validation Failures

**Pattern**:
```json
{
  "detection": {
    "symptom": "Transaction status = 'broadcast' but action didn't occur",
    "diagnosis": {
      "1": "Query game state",
      "2": "Compare expected vs actual",
      "3": "Identify mismatch",
      "4": "Check requirements"
    },
    "solution": {
      "1": "Identify missing requirement",
      "2": "Fix requirement",
      "3": "Retry action",
      "4": "Verify again"
    }
  }
}
```

---

## Best Practices

### 1. Always Verify

**Rule**: Never assume action succeeded based on transaction status alone.

**Practice**: Always query game state after broadcast.

### 2. Define Expected Result

**Rule**: Know what success looks like before acting.

**Practice**: Define expected result, then verify it occurred.

### 3. Check Multiple Indicators

**Rule**: Don't rely on single verification point.

**Practice**: Check multiple indicators (entity exists, state changed, relationships updated).

### 4. Handle Failures Gracefully

**Rule**: Validation failures are expected and normal.

**Practice**: Detect failures, identify cause, fix, retry.

### 5. Use Verification in All Tasks

**Rule**: Verification is part of every action.

**Practice**: Include verification steps in all task workflows.

---

## Related Documentation

- **Transaction Flow**: `transaction-flow.md` - Complete transaction flow
- **Common Failures**: `common-failures.md` - Common validation failures
- **Action Requirements**: `../schemas/actions.json` - Action requirements
- **Task Workflows**: `../tasks/` - Task workflows with verification steps

---

*Last Updated: January 2025*

