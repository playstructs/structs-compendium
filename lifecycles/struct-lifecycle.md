# Struct Lifecycle

**Version**: 1.0.0  
**Category**: Lifecycle  
**Status**: Stable

## Overview

This document describes the complete lifecycle of a Struct entity, from initial creation through destruction. Understanding this lifecycle is essential for planning building operations, managing resources, and avoiding unexpected state changes.

---

## Lifecycle States

### State Diagram

```
[Not Exists]
    ↓
[Initiate Build] → [Materialized] (not built, power reserved)
    ↓
[Complete Build] → [Built] (construction complete)
    ↓
[Activate] → [Online] (active, operational)
    ↓
[Deactivate] → [Offline] (inactive, but built)
    ↓
[Activate] → [Online] (can reactivate)
    ↓
[Destroy/Planet Complete] → [Destroyed] (removed from game)
```

---

## State Definitions

### 1. Materialized (Initiated)

**State**: `isMaterialized = true`, `isBuilt = false`

**Description**: Struct has been initiated but construction is not complete.

**Characteristics**:
- Struct exists in game state
- Power capacity reserved (`BuildDraw`)
- Charge cost paid
- Not functional (cannot perform operations)
- Health set to `structType.GetMaxHealth()`
- `BlockStartBuild` timestamp set

**Actions Available**:
- Complete build (requires proof-of-work)
- Cannot activate (not built yet)
- Cannot perform operations

**Power Consumption**: `BuildDraw` (reserved from player capacity)

**Code Reference**: `x/structs/keeper/msg_server_struct_build_initiate.go`

---

### 2. Built

**State**: `isBuilt = true`, `isOnline = false`

**Description**: Struct construction is complete, but struct is not active.

**Characteristics**:
- Construction complete
- Proof-of-work validated
- Power consumption switches from `BuildDraw` to `PassiveDraw`
- Can be activated
- Still not functional (must be activated)

**Actions Available**:
- Activate (bring online)
- Cannot perform operations (not online)
- Can set defense
- Can move (if applicable)

**Power Consumption**: `PassiveDraw` (when activated)

**Code Reference**: `x/structs/keeper/msg_server_struct_build_complete.go`

---

### 3. Online (Active)

**State**: `isOnline = true`, `isBuilt = true`

**Description**: Struct is active and operational.

**Characteristics**:
- Fully functional
- Can perform operations (mining, refining, combat, etc.)
- Consumes power (`PassiveDraw`)
- Requires charge for operations
- Can be deactivated

**Actions Available**:
- All struct operations (mining, refining, combat, etc.)
- Deactivate (take offline)
- Set/clear defense
- Activate/deactivate stealth
- Move (if applicable)
- Attack (if combat struct)

**Power Consumption**: `PassiveDraw` (active consumption)

**Charge Requirements**: Varies by operation:
- Mining: `oreMiningCharge`
- Refining: `oreRefiningCharge`
- Combat: `attackCharge`
- Activation: `activateCharge`

**Code Reference**: `x/structs/keeper/msg_server_struct_activate.go`

---

### 4. Offline (Inactive)

**State**: `isOnline = false`, `isBuilt = true`

**Description**: Struct is built but not active.

**Characteristics**:
- Construction complete
- Not consuming power (no `PassiveDraw`)
- Cannot perform operations
- Can be reactivated

**Actions Available**:
- Activate (bring online)
- Cannot perform operations (not online)
- Can set defense
- Can move (if applicable)

**Power Consumption**: None (inactive)

**Code Reference**: `x/structs/keeper/msg_server_struct_deactivate.go`

---

### 5. Destroyed

**State**: `isDestroyed = true`

**Description**: Struct has been removed from the game.

**Characteristics**:
- No longer exists in game state
- All attributes cleared
- Power capacity released
- Owner struct count decremented
- Slot on planet/fleet freed
- Queued for cleanup

**Actions Available**: None (struct no longer exists)

**Power Consumption**: None (struct destroyed)

**Code Reference**: `x/structs/keeper/struct_cache.go:DestroyAndCommit`

---

## State Transitions

### Initiate Build

**Action**: `MsgStructBuildInitiate`

**From**: Not Exists  
**To**: Materialized

**Requirements**:
- Player online
- Sufficient resources (charge)
- Valid location (planet or fleet)
- Sufficient power capacity (`BuildDraw`)
- If building on planet:
  - Command Ship online in fleet
  - Fleet on station

**Effects**:
- Struct created in materialized state
- Charge cost paid
- Power capacity reserved (`BuildDraw`)
- `BlockStartBuild` timestamp set
- Health set to max

**Validation**: Transaction may broadcast but struct not created if requirements not met. Always verify game state after broadcast.

---

### Complete Build

**Action**: `MsgStructBuildComplete`

**From**: Materialized  
**To**: Built

**Requirements**:
- Player online
- Struct in materialized state
- Valid proof-of-work (hash + nonce)
- Proof-of-work difficulty met

**Effects**:
- Struct becomes built
- Power consumption switches from `BuildDraw` to `PassiveDraw` (when activated)
- Struct can now be activated

**Validation**: Proof-of-work must be valid. Always verify struct state after completion.

---

### Activate

**Action**: `MsgStructActivate`

**From**: Built (Offline)  
**To**: Online

**Requirements**:
- Player online
- Struct built
- Sufficient charge (`activateCharge`)
- Sufficient power capacity (`PassiveDraw`)

**Effects**:
- Struct becomes online
- Power consumption starts (`PassiveDraw`)
- Struct can perform operations

**Validation**: Transaction may broadcast but struct not activated if requirements not met. Always verify game state after broadcast.

---

### Deactivate

**Action**: `MsgStructDeactivate`

**From**: Online  
**To**: Offline

**Requirements**:
- Player online
- Struct online

**Effects**:
- Struct becomes offline
- Power consumption stops (no `PassiveDraw`)
- Struct cannot perform operations

**Validation**: Always verify struct state after deactivation.

---

### Destroy

**Action**: Explicit destruction or automatic (planet completion)

**From**: Any state (Materialized, Built, Online, Offline)  
**To**: Destroyed

**Triggers**:
- Planet completion (automatic - all structs destroyed)
- Explicit destruction (player action)
- Combat destruction (attack)

**Effects**:
- Struct destroyed
- All attributes cleared
- Power capacity released
- Owner struct count decremented
- Slot on planet/fleet freed

**Validation**: Always verify struct no longer exists after destruction.

---

## Special States

### Stealth Mode

**State**: `isHidden = true`

**Description**: Struct is in stealth mode (hidden from other players).

**Characteristics**:
- Can be activated/deactivated independently of online status
- Requires stealth system capability
- Hidden from other players' view

**Actions**:
- `MsgStructStealthActivate` - Activate stealth
- `MsgStructStealthDeactivate` - Deactivate stealth

**Code Reference**: `x/structs/keeper/msg_server_struct_stealth_*.go`

---

### Locked

**State**: `isLocked = true`

**Description**: Struct is locked and cannot perform actions.

**Characteristics**:
- Cannot perform operations
- Cannot be activated/deactivated
- Cannot move
- Typically temporary state

**Actions Available**: None (struct locked)

---

## Lifecycle Patterns

### Standard Building Pattern

1. **Initiate Build** → Materialized
2. **Complete Build** → Built
3. **Activate** → Online
4. **Perform Operations** → Online
5. **Deactivate** (optional) → Offline
6. **Reactivate** (optional) → Online
7. **Destroy** (eventual) → Destroyed

---

### Planet Completion Pattern

1. **Planet Ore Depleted** → Planet status: "complete"
2. **All Structs Destroyed** → Destroyed (automatic)
3. **All Fleets Sent Away** → Away (automatic)
4. **Planet Cleared** → No structures remain

**⚠️ CRITICAL**: Mining all ore from a planet destroys all structs on that planet automatically.

---

## Power Consumption by State

| State | Power Consumption | Notes |
|-------|-------------------|-------|
| Materialized | `BuildDraw` | Reserved during building |
| Built (Offline) | None | Not active |
| Built (Online) | `PassiveDraw` | Active consumption |
| Destroyed | None | Struct no longer exists |

---

## Charge Costs by Operation

| Operation | Charge Cost | State Required |
|-----------|-------------|----------------|
| Initiate Build | `buildCharge` | Not Exists |
| Complete Build | None (proof-of-work) | Materialized |
| Activate | `activateCharge` | Built (Offline) |
| Deactivate | None | Online |
| Mining | `oreMiningCharge` | Online |
| Refining | `oreRefiningCharge` | Online |
| Combat | `attackCharge` | Online |

---

## Validation Patterns

### Pre-Build Validation

1. Check player online status
2. Verify sufficient resources (charge)
3. Verify valid location
4. Verify sufficient power capacity
5. If building on planet:
   - Verify Command Ship online
   - Verify fleet on station

### Post-Build Verification

1. Query struct state
2. Verify struct exists
3. Verify struct state is Materialized
4. Verify power capacity reserved

### Pre-Activation Validation

1. Check player online status
2. Verify struct is built
3. Verify sufficient charge
4. Verify sufficient power capacity

### Post-Activation Verification

1. Query struct state
2. Verify struct is online
3. Verify power consumption active
4. Verify struct can perform operations

---

## Related Documentation

- **Actions**: `../schemas/actions.json` - Action definitions
- **Entities**: `../schemas/entities.json` - Entity definitions
- **Systems**: `../systems/power-system.md` - Power system
- **Validation**: `../validation/transaction-flow.md` - Validation patterns
- **Tasks**: `../tasks/building.json` - Building workflow

---

## Code References

- **Initiate Build**: `x/structs/keeper/msg_server_struct_build_initiate.go`
- **Complete Build**: `x/structs/keeper/msg_server_struct_build_complete.go`
- **Activate**: `x/structs/keeper/msg_server_struct_activate.go`
- **Deactivate**: `x/structs/keeper/msg_server_struct_deactivate.go`
- **Destroy**: `x/structs/keeper/struct_cache.go:DestroyAndCommit`
- **Struct Cache**: `x/structs/keeper/struct_cache.go`

---

*Last Updated: January 2025*

