# Planet Lifecycle

**Version**: 1.0.0  
**Category**: Lifecycle  
**Status**: Stable

## Overview

This document describes the complete lifecycle of a Planet entity, from exploration through completion. Understanding this lifecycle is essential for planning exploration, managing resources, and avoiding unexpected planet completion.

---

## Lifecycle States

### State Diagram

```
[Not Exists]
    ↓
[Explore Planet] → [Active] (has ore, can be built on)
    ↓
[Mining Operations] → [Active] (ore decreasing)
    ↓
[Ore Depleted] → [Complete] (all structures destroyed, fleet sent away)
```

---

## State Definitions

### 1. Active

**State**: `status = "active"`, `remainingOre > 0`

**Description**: Planet is active and can be built on.

**Characteristics**:
- Has ore remaining (`remainingOre > 0`)
- Can build structs on planet
- Can perform mining operations
- Can have fleets on station
- Can have structs (space, air, land, water)

**Actions Available**:
- Build structs (if fleet on station)
- Mine ore
- Set up infrastructure
- Defend planet
- All planet operations

**Starting Properties** (all newly explored planets):
- `maxOre`: 5
- `spaceSlots`: 4
- `airSlots`: 4
- `landSlots`: 4
- `waterSlots`: 4

**Code Reference**: `x/structs/keeper/planet_cache.go`

---

### 2. Complete

**State**: `status = "complete"`, `remainingOre = 0`

**Description**: Planet has been depleted of all ore.

**Characteristics**:
- No ore remaining (`remainingOre = 0`)
- All structs automatically destroyed
- All fleets automatically sent away
- Cannot build on planet
- Cannot perform operations
- Planet cleared of all structures

**Actions Available**: None (planet complete, cannot build or operate)

**⚠️ CRITICAL**: When a planet runs out of ore:
- **All structs on the planet are automatically destroyed** (space, air, land, water)
- **All fleets are automatically sent away** (peace deal)
- **Planet status changes to "complete"**

**Code Reference**: `x/structs/keeper/planet_cache.go`

---

## State Transitions

### Explore Planet

**Action**: `MsgPlanetExplore`

**From**: Not Exists (or previous planet complete)  
**To**: Active

**Requirements**:
- Player online
- Current planet has 0 ore remaining (if player owns a planet)
- Player can only own one planet at a time

**Effects**:
- New planet created
- Planet status: "active"
- `remainingOre`: 5 (starting ore)
- `spaceSlots`: 4
- `airSlots`: 4
- `landSlots`: 4
- `waterSlots`: 4
- Fleet moves to new planet
- Old planet released (if player owned one)

**Validation**: Transaction may broadcast but planet not created if requirements not met. Always verify game state after broadcast.

**Code Reference**: `x/structs/keeper/msg_server_planet_explore.go`

---

### Ore Depletion

**Action**: Mining operations (automatic when `remainingOre = 0`)

**From**: Active  
**To**: Complete

**Triggers**:
- Mining operations reduce `remainingOre` to 0
- Automatic planet completion

**Effects**:
- Planet status: "complete"
- All structs on planet automatically destroyed
- All fleets automatically sent away
- Planet cleared of all structures

**⚠️ CRITICAL**: This is an automatic process. Once ore reaches 0, planet completion is immediate and irreversible.

**Code Reference**: `x/structs/keeper/planet_cache.go`

---

## Ownership Rules

### One Planet Per Player

**Rule**: Players can only own one planet at a time.

**Implications**:
- Must complete current planet (ore = 0) before exploring new planet
- Cannot own multiple planets simultaneously
- Exploring new planet releases old planet

**Validation**: `MsgPlanetExplore` requires current planet to have 0 ore remaining.

---

### Planet Release

**When**: Player explores new planet

**Effects**:
- Old planet released (no longer owned by player)
- Fleet moves to new planet
- Old planet structures remain (if any ore remaining)
- Old planet can be claimed by other players (if ore remaining)

---

## Critical Mechanics

### Planet Completion System

**⚠️ CRITICAL MECHANIC**: When a planet runs out of ore:

1. **All structs destroyed** (space, air, land, water)
2. **All fleets sent away** (peace deal)
3. **Planet status: "complete"**
4. **Cannot build on planet** (planet complete)

**Impact**: Complete loss of all planet-based structures

**Strategy Options**:
1. **Keep some ore**: Don't mine all ore to prevent completion
2. **Move structs**: Move critical structs before planet depleted
3. **Accept completion**: Plan for planet completion and rebuild on new planet

**Warning**: This is a critical mechanic - players need to understand that depleted planets clear all structures.

---

## Lifecycle Patterns

### Standard Exploration Pattern

1. **Explore Planet** → Active (ore = 5)
2. **Build Infrastructure** → Active (structs built)
3. **Mine Ore** → Active (ore decreasing)
4. **Continue Operations** → Active (ore > 0)
5. **Ore Depleted** → Complete (all structures destroyed)
6. **Explore New Planet** → Active (new planet)

---

### Strategic Ore Management Pattern

1. **Explore Planet** → Active
2. **Build Infrastructure** → Active
3. **Mine Ore Strategically** → Active (keep some ore)
4. **Maintain Planet** → Active (ore > 0, structures preserved)
5. **When Ready**: Mine remaining ore → Complete
6. **Explore New Planet** → Active (new planet)

---

## Resource Management

### Starting Resources

**All newly explored planets start with**:
- `maxOre`: 5
- `spaceSlots`: 4
- `airSlots`: 4
- `landSlots`: 4
- `waterSlots`: 4

**No variation**: All planets start identical

---

### Ore Management

**Ore Types**:
- Alpha Ore (mineable)
- Converted to Alpha Matter (refined)

**Ore Security**:
- Alpha Ore: Can be stolen (vulnerable)
- Alpha Matter: Cannot be stolen (secure)

**Ore Depletion**:
- Mining reduces `remainingOre`
- When `remainingOre = 0`: Planet completes automatically

---

## Validation Patterns

### Pre-Exploration Validation

1. Check player online status
2. Verify current planet has 0 ore (if player owns planet)
3. Verify player doesn't own another active planet

### Post-Exploration Verification

1. Query planet state
2. Verify planet exists
3. Verify planet status is "active"
4. Verify `remainingOre` is 5
5. Verify fleet moved to new planet

### Pre-Mining Validation

1. Check planet status is "active"
2. Verify `remainingOre > 0`
3. Verify mining struct is online
4. Verify sufficient charge

### Ore Depletion Monitoring

1. Monitor `remainingOre` value
2. When `remainingOre = 0`: Planet will complete
3. Plan for planet completion (move structs, prepare for new planet)

---

## Related Documentation

- **Actions**: `../schemas/actions.json` - Action definitions
- **Entities**: `../schemas/entities.json` - Entity definitions
- **Struct Lifecycle**: `../lifecycles/struct-lifecycle.md` - Struct lifecycle
- **Fleet Lifecycle**: `../lifecycles/fleet-lifecycle.md` - Fleet lifecycle
- **Tasks**: `../tasks/exploration.json` - Exploration workflow

---

## Code References

- **Explore Planet**: `x/structs/keeper/msg_server_planet_explore.go`
- **Planet Cache**: `x/structs/keeper/planet_cache.go`
- **Planet Types**: `x/structs/types/keys.go`

---

*Last Updated: January 2025*

