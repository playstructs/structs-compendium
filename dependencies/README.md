# Dependencies

**Version**: 1.0.0  
**Category**: Dependencies  
**Status**: Stable

## Overview

This directory contains dependency chains and requirements for game actions. Understanding dependencies is critical for successful gameplay.

---

## What Are Dependencies?

Dependencies are requirements that must be met before an action can succeed. They form chains where one action must complete before another can begin.

---

## Dependency Files

### Core Dependencies

- **`command-ship.json`** - Command Ship dependency chain (required for planet building)
- **`building.json`** - Building structure dependencies
- **`exploration.json`** - Planet exploration dependencies

---

## Key Dependency Chains

### Command Ship Chain

**Critical**: Command Ship must be built AND online before building on planets.

**Chain**:
1. Build Command Ship in fleet
2. Complete Command Ship build (proof-of-work)
3. Activate Command Ship
4. **Then** can build on planets

**Reference**: `command-ship.json`

---

### Building Dependencies

**For Planet Building**:
- Command Ship online ✅
- Fleet onStation ✅
- Player online ✅
- Sufficient power ✅
- Available slots ✅

**For Fleet Building**:
- Player online ✅
- Sufficient power ✅

**Reference**: `building.json`

---

### Exploration Dependencies

**Requirements**:
- Current planet empty (ore = 0) ✅
- Player online ✅

**Constraint**: One planet per player

**Reference**: `exploration.json`

---

## Using Dependencies

### Before Acting

1. **Check Dependencies**: Review dependency file for action
2. **Verify Requirements**: Check each requirement
3. **Fix Missing**: Address any missing requirements
4. **Then Act**: Perform action once all requirements met

### After Acting

1. **Verify Success**: Check if action succeeded
2. **If Failed**: Review dependencies to identify missing requirement
3. **Fix and Retry**: Address missing requirement, retry action

---

## Common Mistakes

1. **Skipping Dependency Checks**: Not checking requirements before acting
2. **Incomplete Chains**: Not completing full dependency chain (e.g., Command Ship materialized but not online)
3. **Wrong Order**: Attempting actions in wrong order
4. **Assuming Success**: Not verifying dependencies after action

---

## Related Documentation

- **Tasks**: `../tasks/` - Task workflows with dependency steps
- **Validation**: `../validation/` - Validation patterns and common failures
- **Schemas**: `../schemas/actions.json` - Action requirements

---

*Last Updated: December 7, 2025*

