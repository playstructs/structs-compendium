# Common Validation Failures

**Version**: 1.0.0  
**Category**: Validation  
**Status**: Stable

## Overview

This document describes common validation failures, their causes, and solutions. Understanding these patterns helps AI agents handle failures gracefully.

---

## Failure Pattern

### General Pattern

```json
{
  "failurePattern": {
    "symptom": "Transaction broadcasts but action doesn't occur",
    "cause": "Requirements not met",
    "solution": "Check requirements, fix, retry"
  }
}
```

---

## Common Failures

### 1. Planet Building Without Command Ship

**Symptom**: Transaction broadcasts but struct not created

**Cause**: Command Ship not built or not online

**Requirements Check**:
```json
{
  "requirements": {
    "commandShipBuilt": "Check fleet for Command Ship struct",
    "commandShipOnline": "Command Ship status must be 'online' (not just materialized)",
    "fleetOnStation": "Fleet status must be 'onStation' (not 'away')",
    "playerOnline": "Player must be online (not halted)"
  }
}
```

**Dependency Chain**:
```json
{
  "dependencyChain": {
    "1": "Build Command Ship in fleet (MsgStructBuild)",
    "2": "Complete Command Ship build (MsgStructBuildComplete with proof-of-work)",
    "3": "Activate Command Ship (bring online)",
    "4": "Then can build on planets"
  }
}
```

**Solution**:
1. Check if Command Ship exists in fleet
2. If not, build Command Ship first
3. Complete Command Ship build
4. Activate Command Ship
5. Then retry planet building

**Reference**: `tasks/onboarding.json#/steps/3-5`

---

### 2. Exploration Without Completing Current Planet

**Symptom**: Transaction broadcasts but planet ownership unchanged

**Cause**: Current planet still has ore remaining

**Requirements Check**:
```json
{
  "requirements": {
    "currentPlanetEmpty": "Current planet must have 0 ore remaining",
    "playerOnline": "Player must be online"
  }
}
```

**Verification**:
```json
{
  "verification": {
    "checkPlanetOre": "Query planet attributes for ore amount",
    "mustBeZero": "Ore amount must be 0 before exploring",
    "thenExplore": "Once empty, exploration will succeed"
  }
}
```

**Solution**:
1. Query current planet attributes for ore amount
2. If ore > 0, mine all ore first
3. Verify ore amount = 0
4. Then retry exploration

**Reference**: `tasks/exploration.json#/step/1`

---

### 3. Building Without Sufficient Power

**Symptom**: Transaction broadcasts but struct not created

**Cause**: Insufficient power capacity

**Requirements Check**:
```json
{
  "requirements": {
    "sufficientPower": "Must have power capacity > struct passive draw",
    "checkLoad": "Current load + struct load <= capacity",
    "checkStructDraw": "Struct passive draw must be available"
  }
}
```

**Calculation**:
```json
{
  "calculation": {
    "availablePower": "(capacity + capacitySecondary) - (load + structsLoad)",
    "requiredPower": "struct.passiveDraw",
    "check": "availablePower >= requiredPower"
  }
}
```

**Solution**:
1. Query player power capacity and current load
2. Calculate available power
3. Check if available >= struct passive draw
4. If not, increase power capacity or reduce load
5. Then retry building

**Reference**: `tasks/building.json#/step/3`

---

### 4. Building Without Available Slots

**Symptom**: Transaction broadcasts but struct not created

**Cause**: No available slots on planet for struct type

**Requirements Check**:
```json
{
  "requirements": {
    "availableSlots": "Must have available slots for struct ambit",
    "checkAmbit": "Struct ambit must match slot type (space/air/land/water)",
    "checkCount": "Available slots > 0"
  }
}
```

**Calculation**:
```json
{
  "calculation": {
    "availableSlots": "planet.{ambit}Slots - existingStructsInAmbit",
    "check": "availableSlots > 0"
  }
}
```

**Solution**:
1. Query planet attributes for slot counts
2. Query existing structs on planet
3. Calculate available slots for struct ambit
4. If no slots available, choose different planet or remove struct
5. Then retry building

**Reference**: `tasks/building.json#/step/4`

---

### 5. Player Not Online

**Symptom**: Transaction broadcasts but action doesn't occur

**Cause**: Player is halted (offline)

**Requirements Check**:
```json
{
  "requirements": {
    "playerOnline": "Player must be online (not halted)",
    "checkStatus": "Query player status"
  }
}
```

**Solution**:
1. Query player status
2. If player is halted, wait for player to come online
3. Then retry action

**Reference**: `schemas/entities.json#/entities/Player`

---

## Error Detection Patterns

### Pattern 1: Broadcast But No Result

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

### Pattern 2: Requirements Not Documented

```json
{
  "pattern": {
    "symptom": "Action fails but requirements unclear",
    "diagnosis": {
      "1": "Check action requirements in actions.json",
      "2": "Check validation patterns",
      "3": "Query game state to understand constraints"
    },
    "solution": {
      "1": "Document missing requirement",
      "2": "Update actions.json",
      "3": "Add to validation patterns"
    }
  }
}
```

---

## Troubleshooting Steps

### General Troubleshooting

1. **Check Transaction Status**: Did transaction reach "broadcast"?
2. **Query Game State**: Did action actually occur?
3. **Compare Expected vs Actual**: What's different?
4. **Check Requirements**: Which requirement wasn't met?
5. **Fix Requirement**: Address missing requirement
6. **Retry Action**: Submit transaction again
7. **Verify Success**: Confirm action occurred

---

## Prevention

### Pre-Action Checks

Always check requirements before acting:

```json
{
  "preActionChecks": {
    "playerOnline": "Query player status",
    "commandShipOnline": "Query fleet for Command Ship",
    "fleetOnStation": "Query fleet status",
    "sufficientPower": "Calculate available power",
    "availableSlots": "Calculate available slots",
    "currentPlanetEmpty": "Query planet ore amount"
  }
}
```

### Post-Action Verification

Always verify after broadcast:

```json
{
  "postActionVerification": {
    "queryGameState": "Query relevant entities",
    "compareExpected": "Compare expected vs actual",
    "ifMismatch": "Check requirements and retry"
  }
}
```

---

## Related Documentation

- **Transaction Flow**: `transaction-flow.md` - Complete transaction flow
- **Verification**: `verification.md` - How to verify actions
- **Action Requirements**: `../schemas/actions.json` - Action requirements
- **Task Workflows**: `../tasks/` - Task workflows with validation

---

*Last Updated: January 2025*

