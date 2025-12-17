# Player Lifecycle

**Version**: 1.0.0  
**Category**: Lifecycle  
**Status**: Stable

## Overview

This document describes the Player entity lifecycle, focusing on online/halted states. Understanding this lifecycle is essential for planning actions and understanding when players can perform operations.

---

## Lifecycle States

### State Diagram

```
[Online] (active, can perform actions)
    ↓
[Halt] → [Halted] (inactive, cannot perform actions)
    ↓
[Resume] → [Online] (active again)
```

---

## State Definitions

### 1. Online

**State**: `playerOnline = true` or `status = "online"`

**Description**: Player is active and can perform actions.

**Characteristics**:
- Player is active
- Can perform all game actions
- Can build structs
- Can manage resources
- Can participate in combat
- Can explore planets
- Can manage fleets

**Actions Available**:
- All game actions (building, mining, combat, etc.)
- Resource management
- Fleet operations
- Planet operations
- Guild operations

**Requirements**: Player must be online to perform most actions.

**Code Reference**: `x/structs/keeper/player_cache.go`

---

### 2. Halted

**State**: `playerOnline = false` or `status = "halted"`

**Description**: Player is inactive and cannot perform actions.

**Characteristics**:
- Player is inactive
- Cannot perform game actions
- Cannot build structs
- Cannot manage resources
- Cannot participate in combat
- Cannot explore planets
- Cannot manage fleets

**Actions Available**: None (player halted)

**Requirements**: Player must resume to perform actions.

**Code Reference**: `x/structs/keeper/player_cache.go`

---

## State Transitions

### Halt Player

**Action**: Player halt (automatic or manual)

**From**: Online  
**To**: Halted

**Triggers**:
- Automatic (system action)
- Manual (player action)
- Error conditions

**Effects**:
- Player becomes halted
- Cannot perform actions
- Game state preserved

**Validation**: Player status changes to halted.

---

### Resume Player

**Action**: `MsgPlayerResume` (if implemented)

**From**: Halted  
**To**: Online

**Requirements**:
- Player is halted
- Resume action available

**Effects**:
- Player becomes online
- Can perform actions again
- Game state restored

**Validation**: Transaction may broadcast but player not resumed if requirements not met. Always verify game state after broadcast.

**Code Reference**: `x/structs/keeper/msg_server_player_resume.go` (if exists)

---

## Player Online Requirement

### Most Actions Require Online Status

**Rule**: Most game actions require player to be online.

**Actions Requiring Online Status**:
- Building structs
- Mining operations
- Combat operations
- Fleet movements
- Planet exploration
- Resource management
- Guild operations

**Validation Pattern**:
1. Check player online status
2. Verify `playerOnline = true`
3. Then allow action

**Code Reference**: Most action handlers check player online status

---

## Calculated Fields

### Player Online Status

**Field**: `playerOnline`

**Calculation**: Based on player state and conditions

**Formula**: `playerOnline = !isHalted && conditionsMet`

**Usage**: Used to validate action requirements

**Code Reference**: `x/structs/keeper/player_cache.go`

---

## Lifecycle Patterns

### Standard Active Pattern

1. **Player Online** → Can perform actions
2. **Perform Actions** → Game operations
3. **Continue Operations** → Online
4. **Halt** (optional) → Halted
5. **Resume** (optional) → Online

---

### Halted Recovery Pattern

1. **Player Halted** → Cannot perform actions
2. **Resume Player** → Online
3. **Verify Online** → Confirm status
4. **Resume Operations** → Continue actions

---

## Validation Patterns

### Pre-Action Validation

1. Check player online status
2. Verify `playerOnline = true`
3. Then allow action

### Post-Action Verification

1. Query player state
2. Verify player still online
3. Verify action succeeded

### Halted Player Handling

1. Detect player halted
2. Block actions
3. Provide error message
4. Require resume before actions

---

## Related Documentation

- **Actions**: `../schemas/actions.json` - Action definitions
- **Entities**: `../schemas/entities.json` - Entity definitions
- **Validation**: `../validation/transaction-flow.md` - Validation patterns
- **Tasks**: `../tasks/onboarding.json` - Onboarding workflow

---

## Code References

- **Player Cache**: `x/structs/keeper/player_cache.go`
- **Player Resume**: `x/structs/keeper/msg_server_player_resume.go` (if exists)
- **Player Types**: `x/structs/types/player.pb.go`

---

*Last Updated: January 2025*

