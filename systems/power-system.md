# Power System

**Version**: 1.0.0  
**Category**: System  
**Status**: Stable

## Overview

The power system manages energy (Watts) production, consumption, and capacity. Understanding the power system is critical for building and operating structures.

---

## Power Components

### Capacity

**Total Capacity**: `capacity + capacitySecondary`

- **capacity**: Primary power capacity
- **capacitySecondary**: Secondary power capacity (from power structs)

### Load

**Total Load**: `load + structsLoad`

- **load**: Base player load
- **structsLoad**: Load from all online structs

### Available Power

**Formula**: `availablePower = (capacity + capacitySecondary) - (load + structsLoad)`

**Online Requirement**: `(load + structsLoad) <= (capacity + capacitySecondary)`

---

## Power Requirements

### Building Requirements

**Build Power**: Power required during building (BuildDraw)

**Passive Power**: Power required when struct is online (PassiveDraw)

**Total Required**: `buildPower + passivePower`

### Example Power Requirements

**Ore Extractor**:
- Build Power: 500,000 W
- Passive Power: 500,000 W
- Total: 1,000,000 W

**Planetary Defense Cannon**:
- Build Power: 600,000 W
- Passive Power: 600,000 W
- Total: 1,200,000 W

**Ore Bunker**:
- Build Power: 200,000 W
- Passive Power: 200,000 W
- Total: 400,000 W

---

## Power States

### Player Online

**Requirement**: `availablePower > 0`

**If Offline**: Player is halted, cannot perform actions

**Check**: `powerStatus.playerOnline == true`

### Struct Online

**Requirement**: `availablePower >= struct.passiveDraw`

**If Offline**: Struct cannot operate

**Check**: `struct.status == 'online'`

---

## Power Management

### Increasing Capacity

1. Build power generation structs (Field Generator, Continental Power Plant, World Engine)
2. Connect to substations
3. Allocate from reactors

### Reducing Load

1. Deactivate structs (turns off passive draw)
2. Reduce active operations
3. Optimize struct usage

### Monitoring

**Metrics**:
- Current capacity
- Current load
- Available power
- Power consumption rate

**Check Frequency**: Before building, after building, periodically

---

## Common Issues

### Insufficient Power Capacity

**Symptom**: Cannot build or operate structs

**Solution**:
1. Increase power capacity (build power structs)
2. Reduce load (deactivate structs)
3. Verify available power > 0

### Player Offline

**Symptom**: Player is halted, cannot perform actions

**Solution**:
1. Check power capacity vs load
2. Increase capacity or reduce load
3. Verify player online

---

## Related Documentation

- **Tasks**: `../tasks/resource-management.json` - Resource management
- **Dependencies**: `../dependencies/building.json` - Building dependencies
- **Troubleshooting**: `../troubleshooting/common-issues.md` - Common issues

---

*Last Updated: January 2025*

