# Entity Lifecycles

**Version**: 1.0.0  
**Category**: Lifecycle  
**Status**: Stable

## Overview

This directory contains lifecycle documentation for game entities. Understanding entity lifecycles is essential for planning actions, managing resources, and avoiding unexpected state changes.

---

## Lifecycle Documentation

### Core Entity Lifecycles

- **`struct-lifecycle.md`** - Struct lifecycle (materialized → built → online/offline → destroyed)
- **`planet-lifecycle.md`** - Planet lifecycle (exploration → active → complete)
- **`fleet-lifecycle.md`** - Fleet lifecycle (station ↔ away transitions)
- **`player-lifecycle.md`** - Player lifecycle (online/halted states)

---

## Lifecycle Overview

### Struct Lifecycle

**Purpose**: Understand how structs progress from creation to destruction

**Key States**:
- Materialized (initiated, not built)
- Built (construction complete)
- Online/Offline (active/inactive)
- Destroyed (removed from game)

**Reference**: `struct-lifecycle.md`

---

### Planet Lifecycle

**Purpose**: Understand how planets progress from exploration to completion

**Key States**:
- Exploration (being explored)
- Active (has ore, can be built on)
- Complete (ore depleted, all structures destroyed)

**Reference**: `planet-lifecycle.md`

---

### Fleet Lifecycle

**Purpose**: Understand fleet status transitions

**Key States**:
- On Station (at planet, can build)
- Away (away from planet, can raid)

**Reference**: `fleet-lifecycle.md`

---

### Player Lifecycle

**Purpose**: Understand player state management

**Key States**:
- Online (active, can perform actions)
- Halted (inactive, cannot perform actions)

**Reference**: `player-lifecycle.md`

---

## Using Lifecycle Documentation

### Before Acting

1. **Understand Lifecycle**: Review relevant entity lifecycle
2. **Check Current State**: Verify entity is in expected state
3. **Plan Transitions**: Understand what actions cause state changes
4. **Then Act**: Perform action with lifecycle awareness

### After Acting

1. **Verify State Change**: Check entity state changed as expected
2. **Monitor Lifecycle**: Track entity progression through lifecycle
3. **Plan Next Steps**: Use lifecycle knowledge for future actions

---

## Critical Lifecycle Events

### Planet Completion

**⚠️ CRITICAL**: When a planet runs out of ore:
- **All structs on the planet are automatically destroyed**
- **All fleets are automatically sent away**
- **Planet status changes to "complete"**

**Impact**: Complete loss of all planet-based structures

**Strategy**: Plan ore management to prevent unexpected planet completion

---

### Struct Destruction

**Triggers**:
- Planet completion (automatic)
- Explicit destruction (player action)
- Combat destruction (attack)

**Impact**: Complete loss of struct and all associated resources

**Strategy**: Protect critical structs, plan for destruction scenarios

---

## Related Documentation

- **Systems**: `../systems/` - System documentation
- **Tasks**: `../tasks/` - Task workflows
- **Dependencies**: `../dependencies/` - Dependency chains
- **Validation**: `../validation/` - Validation patterns

---

*Last Updated: January 2025*

