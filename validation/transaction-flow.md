# Transaction Flow

**Version**: 1.0.0  
**Category**: Validation  
**Status**: Stable

## Overview

This document describes the complete transaction validation flow for AI agents. Understanding this flow is critical for correctly handling action results.

---

## Complete Transaction Flow

### Step-by-Step Flow

```json
{
  "transactionFlow": {
    "1": "Create transaction (pending)",
    "2": "Sign transaction with player key",
    "3": "Submit to POST /cosmos/tx/v1beta1/txs",
    "4": "Transaction moves to 'broadcast' status",
    "5": "on-chain validation occurs (checks requirements)",
    "6": "Action succeeds OR fails based on validation",
    "7": "Query game state to verify action actually occurred"
  }
}
```

### Visual Flow

```
[Create] → [Sign] → [Submit] → [Broadcast] → [Validate] → [Succeed/Fail] → [Verify]
   ↓         ↓         ↓           ↓             ↓            ↓              ↓
 pending   signed   submitted   broadcast   validating   result        game state
```

---

## Critical Understanding

### ⚠️ IMPORTANT: Broadcast ≠ Success

**A transaction showing "broadcast" status does NOT mean the action succeeded!**

**Why**: Validation happens on-chain AFTER broadcast. If requirements aren't met, the transaction will broadcast but the action won't happen.

### Example

```json
{
  "scenario": "Building struct on planet",
  "transaction": {
    "status": "broadcast",
    "action": "MsgStructBuild"
  },
  "result": {
    "structCreated": false,
    "reason": "Command Ship not online"
  },
  "lesson": "Always verify game state after broadcast!"
}
```

---

## Transaction Status Values

### Status Definitions

```json
{
  "transactionStatus": {
    "pending": {
      "description": "Transaction created, waiting to be processed",
      "action": "wait"
    },
    "broadcast": {
      "description": "Transaction sent to blockchain",
      "action": "wait for validation",
      "warning": "Does NOT mean action succeeded!"
    },
    "confirmed": {
      "description": "Transaction included in block",
      "action": "verify game state"
    },
    "failed": {
      "description": "Transaction failed validation",
      "action": "check requirements and retry"
    }
  }
}
```

---

## Validation Process

### On-chain validation

After broadcast, the blockchain validates:

1. **Player Status**: Is player online? (not halted)
2. **Resource Requirements**: Sufficient resources? (power, Alpha Matter, etc.)
3. **Entity Requirements**: Valid entities? (Command Ship online, fleet onStation, etc.)
4. **State Requirements**: Valid game state? (current planet empty, slots available, etc.)
5. **Proof-of-Work**: Valid proof? (if required)

### Validation Result

```json
{
  "validationResult": {
    "transactionStatus": "broadcast",
    "actionSucceeded": false,
    "reason": "Command Ship not online",
    "gameStateCheck": {
      "expected": "Struct created",
      "actual": "No struct found",
      "match": false
    }
  }
}
```

---

## Verification Pattern

### Always Verify Game State

**Pattern**:
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

### Example Verification

```json
{
  "example": {
    "action": "Build struct on planet",
    "afterBroadcast": {
      "check": "Query struct entity",
      "endpoint": "/structs/struct",
      "filter": "By location_id and struct_type",
      "expected": "Struct exists",
      "actual": "No struct found",
      "result": "Validation failed - check requirements"
    }
  }
}
```

---

## Common Validation Scenarios

### Scenario 1: Planet Building

**Flow**:
1. Submit `MsgStructBuild` transaction
2. Transaction broadcasts
3. on-chain validation checks:
   - Command Ship online? ✅/❌
   - Fleet onStation? ✅/❌
   - Sufficient power? ✅/❌
   - Available slots? ✅/❌
4. If all checks pass: Struct created ✅
5. If any check fails: Struct not created ❌
6. **Always verify**: Query struct entity to confirm

### Scenario 2: Exploration

**Flow**:
1. Submit `MsgPlanetExplore` transaction
2. Transaction broadcasts
3. on-chain validation checks:
   - Current planet empty? ✅/❌
   - Player online? ✅/❌
4. If all checks pass: New planet created ✅
5. If any check fails: Planet ownership unchanged ❌
6. **Always verify**: Query player planet_id to confirm

---

## Best Practices

### 1. Check Requirements Before Acting

**Pattern**:
```json
{
  "preActionCheck": {
    "1": "Query current game state",
    "2": "Verify all requirements met",
    "3": "If not met, fix requirements first",
    "4": "Then perform action"
  }
}
```

### 2. Always Verify After Broadcast

**Pattern**:
```json
{
  "postActionCheck": {
    "1": "Wait for broadcast status",
    "2": "Query game state",
    "3": "Compare expected vs actual",
    "4": "If mismatch, check requirements and retry"
  }
}
```

### 3. Handle Validation Failures Gracefully

**Pattern**:
```json
{
  "failureHandling": {
    "1": "Detect validation failure (action didn't occur)",
    "2": "Identify missing requirement",
    "3": "Fix requirement",
    "4": "Retry action",
    "5": "Verify success"
  }
}
```

---

## Error Detection

### Pattern: Broadcast But No Result

```json
{
  "pattern": {
    "symptom": "Transaction status = 'broadcast' but action didn't occur",
    "diagnosis": {
      "1": "Query game state",
      "2": "Compare expected vs actual",
      "3": "Identify missing requirement",
      "4": "Check requirements list"
    },
    "solution": {
      "1": "Fix missing requirement",
      "2": "Retry action",
      "3": "Verify success"
    }
  }
}
```

---

## Related Documentation

- **Verification**: `verification.md` - How to verify actions
- **Common Failures**: `common-failures.md` - Common validation failures
- **Action Requirements**: `../schemas/actions.json` - Action requirements
- **Validation Patterns**: `../patterns/validation-patterns.md` - Detailed patterns

---

*Last Updated: December 7, 2025*

