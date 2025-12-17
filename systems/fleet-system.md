# Fleet System

**Version**: 1.0.0  
**Category**: System  
**Status**: Stable

## Overview

The fleet system manages player fleets, which contain mobile structs (Command Ships, combat units). Understanding fleet status is critical for building on planets.

---

## Fleet Components

### Fleet Structure

**Owner**: Each player has one fleet

**Fleet ID**: Matches player pattern (e.g., player `1-18` has fleet `18-18`)

**Contents**: Mobile structs (Command Ship, combat units)

### Fleet Status

**onStation**: Fleet is at planet (can build on planets)

**away**: Fleet is away from planet (cannot build on planets)

---

## Fleet Requirements

### For Planet Building

**Requirement**: Fleet must be `onStation`

**Check**: `fleet.status == 'onStation'`

**If Away**: Cannot build on planets

**Solution**: Move fleet back to planet

### Command Ship Requirement

**Requirement**: Command Ship must be in fleet AND online

**Check**: Query fleet for Command Ship struct, verify status = 'online'

**If Missing**: Build Command Ship in fleet first

**If Offline**: Complete and activate Command Ship

---

## Fleet Operations

### Building in Fleet

**Location Type**: `locationType = 2` (fleet)

**Requirements**:
- Player online
- Sufficient power
- Valid struct type for fleet

**Examples**: Command Ship, combat units

### Building on Planets

**Location Type**: `locationType = 1` (planet)

**Requirements**:
- Command Ship online ✅
- Fleet onStation ✅
- Player online ✅
- Sufficient power ✅
- Available slots ✅

**Note**: Cannot build on planets if fleet is away

---

## Fleet Movement

### Automatic Movement

**On Exploration**: Fleet automatically moves to new planet

**On Planet Claim**: Fleet moves to claimed planet

### Manual Movement

**Action**: Move fleet (if available)

**Purpose**: Strategic positioning, combat

---

## Common Issues

### Fleet Away

**Symptom**: Cannot build on planets

**Cause**: Fleet status = 'away'

**Solution**: Move fleet back to planet (onStation)

### Command Ship Missing

**Symptom**: Cannot build on planets

**Cause**: No Command Ship in fleet

**Solution**: Build Command Ship in fleet first

### Command Ship Offline

**Symptom**: Cannot build on planets

**Cause**: Command Ship not online

**Solution**: Complete and activate Command Ship

---

## Related Documentation

- **Dependencies**: `../dependencies/command-ship.json` - Command Ship dependency
- **Dependencies**: `../dependencies/building.json` - Building dependencies
- **Tasks**: `../tasks/onboarding.json` - Onboarding workflow
- **Troubleshooting**: `../troubleshooting/common-issues.md` - Common issues

---

*Last Updated: January 2025*

