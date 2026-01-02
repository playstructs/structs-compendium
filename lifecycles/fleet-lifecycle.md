# Fleet Lifecycle

**Version**: 1.1.0  
**Category**: Lifecycle  
**Status**: Stable  
**Last Updated**: January 1, 2026

## Overview

This document describes the Fleet entity lifecycle, focusing on status transitions between "on station" and "away". Understanding this lifecycle is essential for planning building operations, raids, and fleet movements.

---

## Lifecycle States

### State Diagram

```
[On Station] (at planet, can build)
    ↓
[Move Fleet] → [Away] (away from planet, can raid)
    ↓
[Move Fleet] → [On Station] (back at planet, can build)
```

---

## State Definitions

### 1. On Station

**State**: `status = "station"` or `status = "onStation"`

**Description**: Fleet is at a planet and can build on that planet.

**Characteristics**:
- Fleet is at a planet
- Can build structs on the planet
- Command Ship must be online (for building)
- Cannot raid (fleet is at planet)
- Can perform planet operations

**Actions Available**:
- Build structs on planet (if Command Ship online)
- Move fleet away
- Perform planet operations
- Cannot raid (fleet at planet)

**Requirements for Building**:
- Fleet status: "on station"
- Command Ship online in fleet
- Valid planet location

**Code Reference**: `x/structs/keeper/fleet_cache.go`

---

### 2. Away

**State**: `status = "away"`

**Description**: Fleet is away from planet and can raid.

**Characteristics**:
- Fleet is away from planet
- Can raid planets
- Cannot build on planets
- Can move back to station
- Can perform raid operations

**Actions Available**:
- Raid planets
- Move fleet to station
- Cannot build on planets (fleet away)

**Requirements for Raiding**:
- Fleet status: "away"
- Valid target planet
- Sufficient charge

**Code Reference**: `x/structs/keeper/fleet_cache.go`

---

## State Transitions

### Move Fleet

**Action**: `MsgFleetMove`

**From**: On Station or Away  
**To**: Away or On Station

**Requirements**:
- Player online
- Command Ship online in fleet (for movement)
- Valid destination
- Destination exists and is accessible

**Effects**:
- Fleet status changes
- Fleet location updates
- Building capability changes (on station only)
- Raiding capability changes (away only)

**Validation**: Transaction may broadcast but fleet not moved if requirements not met. Always verify game state after broadcast.

**v0.8.0-beta Improvements**:
- Fleet movement logic has been improved and bug fixes applied
- More reliable fleet status transitions
- Better validation of movement requirements
- Improved error handling for invalid movements

**Code Reference**: `x/structs/keeper/msg_server_fleet_move.go`

---

### Automatic Fleet Movement

**Trigger**: Planet completion

**From**: On Station  
**To**: Away

**Effects**:
- Fleet automatically sent away when planet completes
- Fleet cannot return to completed planet
- Fleet can move to new planet

**⚠️ CRITICAL**: When a planet runs out of ore, all fleets are automatically sent away (peace deal).

---

## Fleet Operations by Status

### On Station Operations

**Available**:
- Build structs on planet
- Perform planet operations
- Manage planet infrastructure
- Set up defenses

**Not Available**:
- Raid planets (fleet at planet)

**Requirements**:
- Command Ship online (for building)
- Fleet on station
- Valid planet location

---

### Away Operations

**Available**:
- Raid planets
- Move to new planet
- Combat operations

**Not Available**:
- Build on planets (fleet away)

**Requirements**:
- Fleet away
- Valid target for operations

---

## Command Ship Requirement

### Building Requirement

**Rule**: Command Ship must be online in fleet to build on planets.

**When**: Fleet is on station and player wants to build structs

**Validation**:
1. Check fleet status is "on station"
2. Query fleet for Command Ship struct
3. Verify Command Ship status is "online"
4. Then allow building

**Code Reference**: `x/structs/keeper/msg_server_struct_build_initiate.go`

---

### Movement Requirement

**Rule**: Command Ship must be online in fleet to move fleet.

**When**: Player wants to move fleet

**Validation**:
1. Query fleet for Command Ship struct
2. Verify Command Ship status is "online"
3. Then allow movement

**Code Reference**: `x/structs/keeper/msg_server_fleet_move.go`

---

## Lifecycle Patterns

### Standard Building Pattern

1. **Fleet On Station** → Can build
2. **Command Ship Online** → Building enabled
3. **Build Structs** → Structures on planet
4. **Continue Operations** → On station
5. **Move Fleet** (optional) → Away

---

### Raiding Pattern

1. **Fleet Away** → Can raid
2. **Raid Planet** → Combat operations
3. **Complete Raid** → Resources gained
4. **Move Fleet** (optional) → On station

---

### Planet Completion Pattern

1. **Fleet On Station** → At planet
2. **Planet Ore Depleted** → Planet completes
3. **Fleet Automatically Sent Away** → Away (automatic)
4. **Fleet Can Move to New Planet** → On station (new planet)

---

## Fleet Structure

### Fleet Components

**Fleet Contains**:
- Command Ship (required for building and movement)
- Combat structs (for raiding)
- Other structs (as needed)

**Fleet Slots**:
- Limited number of struct slots
- Structs occupy slots
- Command Ship occupies one slot

---

## Validation Patterns

### Pre-Building Validation

1. Check fleet status is "on station"
2. Query fleet for Command Ship
3. Verify Command Ship is online
4. Verify valid planet location
5. Then allow building

### Pre-Movement Validation

1. Check player online status
2. Query fleet for Command Ship
3. Verify Command Ship is online
4. Verify valid destination
5. Then allow movement

### Post-Movement Verification

1. Query fleet state
2. Verify fleet status changed
3. Verify fleet location updated
4. Verify building/raiding capability matches status

---

## Related Documentation

- **Actions**: `../schemas/actions.json` - Action definitions
- **Entities**: `../schemas/entities.json` - Entity definitions
- **Systems**: `../systems/fleet-system.md` - Fleet system
- **Struct Lifecycle**: `../lifecycles/struct-lifecycle.md` - Struct lifecycle
- **Planet Lifecycle**: `../lifecycles/planet-lifecycle.md` - Planet lifecycle
- **Tasks**: `../tasks/exploration.json` - Exploration workflow

---

## Code References

- **Move Fleet**: `x/structs/keeper/msg_server_fleet_move.go`
- **Fleet Cache**: `x/structs/keeper/fleet_cache.go`
- **Fleet Types**: `x/structs/types/keys.go`

---

*Last Updated: January 1, 2026*

