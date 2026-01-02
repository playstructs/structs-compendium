# Migration Guide: v0.7.0-beta → v0.8.0-beta

**Version**: 1.0.0  
**Category**: Migration Guide  
**Status**: Stable  
**Last Updated**: January 1, 2026  
**Target Version**: v0.8.0-beta

## Overview

This guide helps AI agents and developers migrate from v0.7.0-beta to v0.8.0-beta. It covers breaking changes, new features, deprecated functionality, and provides code migration examples.

---

## Table of Contents

1. [Breaking Changes](#breaking-changes)
2. [New Features to Adopt](#new-features-to-adopt)
3. [Deprecated Features](#deprecated-features)
4. [Code Migration Examples](#code-migration-examples)
5. [Database Schema Changes](#database-schema-changes)
6. [Testing Checklist](#testing-checklist)
7. [Rollback Plan](#rollback-plan)

---

## Breaking Changes

### 1. Struct Destruction Behavior Change

**Change**: Structs now persist for 5 blocks after destruction (`StructSweepDelay`)

**Impact**: 
- Slots may appear occupied for 5 blocks after struct destruction
- Planet/fleet slot arrays may still reference destroyed struct ID during delay
- Queries must filter by `destroyed = false` for active structs

**Migration**:
```json
{
  "before": {
    "query": "GET /structs/struct?ownerId=1-11",
    "assumption": "All returned structs are active"
  },
  "after": {
    "query": "GET /structs/struct?ownerId=1-11",
    "filter": "Filter results where destroyed == false",
    "note": "Account for 5-block delay when checking slot availability"
  }
}
```

**Reference**: `lifecycles/struct-lifecycle.md#/structsweepdelay`, `troubleshooting/edge-cases.md`

---

### 2. Reactor Staking Management Change

**Change**: Reactor staking is now managed at player level, not per reactor

**Impact**:
- Staking operations affect player-level state
- Individual reactors may show different delegation statuses
- Validation delegation abstracted via Reactor Infuse/Defuse actions

**Migration**:
```json
{
  "before": {
    "approach": "Per-reactor staking management",
    "query": "Check each reactor individually"
  },
  "after": {
    "approach": "Player-level staking management",
    "query": "Query player reactors: GET /structs/reactor?ownerId={playerId}",
    "note": "Account for player-level state affecting all reactors"
  }
}
```

**Reference**: `schemas/entities/reactor.json`, `protocols/economic-protocol.md#/reactor-staking`

---

### 3. Permission System Enhancement

**Change**: Hash permission (bit value 64) added to permission system

**Impact**:
- Permission values are bit-based flags (0-127 range)
- Hash permission bit (64) must be included for Hash permission
- Database has `permission_hash` level (maps to bit 64 in API)

**Migration**:
```json
{
  "before": {
    "permissionCheck": "Check permission.value == 127",
    "assumption": "All permissions included"
  },
  "after": {
    "permissionCheck": "(permissionValue & 64) == 64",
    "grantHash": "newValue = oldValue | 64",
    "note": "Use bitwise operations for permission checks"
  }
}
```

**Reference**: `schemas/game-state.json#/definitions/Permission`, `troubleshooting/permission-issues.md`

---

### 4. Database Schema Changes

**Change**: Multiple database schema changes require query updates

**Impact**:
- `struct.destroyed` column added (must filter queries)
- `permission_hash` level added to permission view
- `signer_tx` table updated for Hash permission support
- Deprecated charge columns removed from `struct_type`

**Migration**:
```sql
-- Before: No destroyed filter
SELECT * FROM struct WHERE owner_id = '1-11';

-- After: Filter destroyed structs
SELECT * FROM struct 
WHERE owner_id = '1-11' 
AND destroyed = false;
```

**Reference**: `schemas/database-schema.json`, `troubleshooting/edge-cases.md`

---

## New Features to Adopt

### 1. Reactor Staking & Validation Delegation

**Feature**: Reactor staking functionality with validation delegation

**Actions**:
- `reactor-infuse` - Handles both energy production and validation delegation
- `reactor-defuse` - Handles both energy removal and validation undelegation
- `reactor-begin-migration` - Begin redelegation process
- `reactor-cancel-defusion` - Cancel undelegation process

**Adoption**:
```json
{
  "example": {
    "action": "reactor-infuse",
    "reactorId": "3-1",
    "alphaMatterAmount": "100000000",
    "note": "Infuse handles both energy production and validation delegation"
  }
}
```

**Reference**: `protocols/economic-protocol.md#/reactor-staking`, `schemas/actions.json`

---

### 2. Hash Permission

**Feature**: Hash permission (bit value 64) for transaction signing

**Adoption**:
```json
{
  "grantHashPermission": {
    "permissionId": "0-1@1-11",
    "currentValue": 63,
    "newValue": "63 | 64 = 127",
    "check": "(127 & 64) == 64"
  }
}
```

**Reference**: `schemas/game-state.json#/definitions/Permission`, `troubleshooting/permission-issues.md`

---

### 3. New Raid Status: attackerRetreated

**Feature**: New raid outcome status for attacker retreats

**Adoption**:
```json
{
  "raidStatus": {
    "before": ["victory", "defeat", "ongoing"],
    "after": ["victory", "defeat", "ongoing", "attackerRetreated"],
    "handling": "Check for attackerRetreated status in raid outcomes"
  }
}
```

**Reference**: `schemas/gameplay.json`, `protocols/gameplay-protocol.md`

---

### 4. Struct Destroyed Field

**Feature**: `destroyed` field added to struct entity

**Adoption**:
```json
{
  "queryStruct": {
    "endpoint": "GET /structs/struct/{structId}",
    "check": "struct.destroyed == false for active structs",
    "filter": "Filter queries by destroyed == false"
  }
}
```

**Reference**: `schemas/entities/struct.json`, `lifecycles/struct-lifecycle.md`

---

## Deprecated Features

### 1. Deprecated Actions

**Deprecated Actions** (use alternatives instead):

- ⚠️ `reactor-allocate` → Use allocation system instead
- ⚠️ `substation-connect` → Use `MsgSubstationAllocationConnect` instead
- ⚠️ `agreement-create` → Use `agreement-open` instead
- ⚠️ `ore-mining` → Use `struct-ore-miner-complete` instead
- ⚠️ `ore-refining` → Use `struct-ore-refinery-complete` instead
- ⚠️ `generator-allocate` → Use `struct-generator-infuse` instead

**Migration**:
```json
{
  "before": {
    "action": "reactor-allocate",
    "reactorId": "3-1",
    "alphaMatterAmount": "100000000"
  },
  "after": {
    "action": "reactor-infuse",
    "reactorId": "3-1",
    "alphaMatterAmount": "100000000",
    "note": "reactor-infuse handles both energy and validation delegation"
  }
}
```

**Reference**: `reference/action-quick-reference.md`

---

### 2. Deprecated Database Columns

**Removed Columns**:
- Deprecated charge columns removed from `struct_type` table (2025-12-08)
- Removed from views and cache triggers

**Migration**:
```sql
-- Before: Used deprecated charge columns
SELECT charge_mine, charge_refine FROM struct_type WHERE id = 14;

-- After: Use new charge system
-- Charge information available through other means
```

**Reference**: `schemas/database-schema.json#/v0.8.0-beta/changes`

---

## Code Migration Examples

### Example 1: Struct Query with Destroyed Filter

**Before (v0.7.0-beta)**:
```json
{
  "query": "GET /structs/struct?ownerId=1-11",
  "assumption": "All structs in response are active",
  "slotCheck": "Check if slot is available immediately after destruction"
}
```

**After (v0.8.0-beta)**:
```json
{
  "query": "GET /structs/struct?ownerId=1-11",
  "filter": {
    "destroyed": false,
    "note": "Filter destroyed structs from results"
  },
  "slotCheck": {
    "delay": "Wait 5 blocks after destruction",
    "verify": "Check struct.destroyed == false before using slot"
  }
}
```

---

### Example 2: Reactor Staking Query

**Before (v0.7.0-beta)**:
```json
{
  "approach": "Query individual reactors",
  "queries": [
    "GET /structs/reactor/3-1",
    "GET /structs/reactor/3-2",
    "GET /structs/reactor/3-3"
  ]
}
```

**After (v0.8.0-beta)**:
```json
{
  "approach": "Batch query player reactors",
  "query": "GET /structs/reactor?ownerId=1-11",
  "check": {
    "staking": "reactor.staking.staked",
    "delegationStatus": "reactor.staking.delegationStatus",
    "delegationAmount": "reactor.staking.delegationAmount"
  },
  "note": "Staking managed at player level"
}
```

---

### Example 3: Permission Check with Hash Permission

**Before (v0.7.0-beta)**:
```json
{
  "permissionCheck": {
    "value": 127,
    "assumption": "All permissions included"
  }
}
```

**After (v0.8.0-beta)**:
```json
{
  "permissionCheck": {
    "value": 127,
    "hashPermission": "(value & 64) == 64",
    "grantHash": "newValue = oldValue | 64",
    "removeHash": "newValue = oldValue & ~64"
  }
}
```

---

### Example 4: Raid Status Handling

**Before (v0.7.0-beta)**:
```json
{
  "raidStatuses": ["victory", "defeat", "ongoing"],
  "handling": "Check for victory or defeat"
}
```

**After (v0.8.0-beta)**:
```json
{
  "raidStatuses": ["victory", "defeat", "ongoing", "attackerRetreated"],
  "handling": {
    "attackerRetreated": "No resources gained or lost",
    "check": "Handle attackerRetreated status in raid outcomes"
  }
}
```

---

### Example 5: Database Query with Destroyed Filter

**Before (v0.7.0-beta)**:
```sql
SELECT * FROM struct WHERE owner_id = '1-11';
-- Returns all structs including destroyed
```

**After (v0.8.0-beta)**:
```sql
SELECT * FROM struct 
WHERE owner_id = '1-11' 
AND destroyed = false;
-- Only returns active structs
```

---

## Database Schema Changes

### Summary of Changes

1. **Struct Table**:
   - Added `destroyed` column (2025-12-29)
   - Tracks struct destruction status
   - Related to StructSweepDelay (5 blocks)

2. **Struct Type Table**:
   - Added `cheatsheet_details` column (2025-11-29)
   - Added `cheatsheet_extended_details` column (2025-12-02)
   - Removed deprecated charge columns (2025-12-08)

3. **Permission View**:
   - Added `permission_hash` level (2025-12-18)
   - Maps to Hash permission bit (64) in API layer

4. **Signer TX Table**:
   - Hash Complete nonces: INTEGER → CHARACTER VARYING (2025-12-15)
   - Updated permission levels for Hash permission (2025-12-18)
   - Added new transaction types (2025-12-18)

5. **Player Address Pending Table**:
   - Added `permissions` column (2025-12-29)

**Reference**: `schemas/database-schema.json` for complete change log

---

## Testing Checklist

### Pre-Migration Testing

- [ ] Review all deprecated actions in codebase
- [ ] Identify all struct queries that need destroyed filter
- [ ] Identify all permission checks that need Hash permission support
- [ ] Review database queries for schema changes
- [ ] Check raid status handling for attackerRetreated

### Migration Testing

- [ ] Test struct queries with destroyed filter
- [ ] Test reactor staking queries and operations
- [ ] Test permission checks with Hash permission
- [ ] Test raid status handling with attackerRetreated
- [ ] Test database queries with new schema
- [ ] Verify slot availability after struct destruction (5-block delay)
- [ ] Test reactor infuse/defuse with validation delegation
- [ ] Test permission bit manipulation operations

### Post-Migration Testing

- [ ] Verify all deprecated actions replaced
- [ ] Verify struct queries filter destroyed structs
- [ ] Verify permission checks include Hash permission
- [ ] Verify raid status handling includes attackerRetreated
- [ ] Verify database queries use new schema
- [ ] Verify slot availability checks account for delay
- [ ] Verify reactor staking operations work correctly
- [ ] Verify permission bit operations work correctly

### Regression Testing

- [ ] Test all existing functionality still works
- [ ] Test error handling for new edge cases
- [ ] Test performance with new queries
- [ ] Test caching strategies with new fields
- [ ] Test rate limiting with new queries

---

## Rollback Plan

### If Migration Fails

1. **Immediate Rollback**:
   - Revert to v0.7.0-beta codebase
   - Restore previous database schema if needed
   - Verify all functionality restored

2. **Partial Rollback**:
   - Keep new features disabled
   - Use deprecated actions temporarily
   - Plan gradual migration

3. **Data Migration**:
   - Backup database before migration
   - Document all schema changes
   - Plan rollback data migration if needed

### Rollback Considerations

- **Struct Destroyed Field**: May need to handle missing field in v0.7.0-beta
- **Permission Hash**: May need to ignore permission_hash in v0.7.0-beta
- **Reactor Staking**: May need to disable staking features in v0.7.0-beta
- **Deprecated Actions**: May need to re-enable deprecated actions temporarily

---

## Migration Timeline

### Recommended Migration Steps

1. **Week 1: Preparation**
   - Review breaking changes
   - Identify all code that needs updates
   - Create migration plan
   - Set up testing environment

2. **Week 2: Code Updates**
   - Update deprecated actions
   - Add destroyed field filters
   - Update permission checks
   - Update database queries

3. **Week 3: Testing**
   - Run test checklist
   - Verify all functionality
   - Performance testing
   - Edge case testing

4. **Week 4: Deployment**
   - Deploy to staging
   - Monitor for issues
   - Deploy to production
   - Monitor post-deployment

---

## Common Migration Issues

### Issue 1: Struct Queries Return Destroyed Structs

**Problem**: Queries return destroyed structs causing incorrect behavior

**Solution**: Always filter by `destroyed = false`

**Reference**: `troubleshooting/edge-cases.md#/edge-case-7`

---

### Issue 2: Slot Appears Occupied After Destruction

**Problem**: Slot appears occupied for 5 blocks after struct destruction

**Solution**: Wait 5 blocks before using slot, check `struct.destroyed == false`

**Reference**: `troubleshooting/edge-cases.md#/edge-case-7`

---

### Issue 3: Permission Check Fails

**Problem**: Permission check fails even though permission should be granted

**Solution**: Use bitwise operations: `(value & 64) == 64` for Hash permission

**Reference**: `troubleshooting/permission-issues.md#/issue-1`

---

### Issue 4: Reactor Staking Status Unclear

**Problem**: Not sure if delegation is active after infuse

**Solution**: Query reactor after infuse/defuse, check `reactor.staking.delegationStatus`

**Reference**: `troubleshooting/reactor-staking-issues.md#/issue-3`

---

## Related Documentation

- **Breaking Changes**: See [Breaking Changes](#breaking-changes) section above
- **New Features**: `CHANGELOG.md` - Complete v0.8.0-beta changes
- **Troubleshooting**: `troubleshooting/` - Troubleshooting guides
- **Edge Cases**: `troubleshooting/edge-cases.md` - Edge cases and gotchas
- **Performance**: `patterns/performance-optimization.md` - Performance tips
- **Database Schema**: `schemas/database-schema.json` - Database changes

---

## Support

**Migration Issues**:
- Check `troubleshooting/` for common issues
- Review `troubleshooting/edge-cases.md` for edge cases
- Check `schemas/errors.json` for error codes

**Questions**:
- Review relevant protocol documentation
- Check pattern documentation
- Review workflow examples

---

*Last Updated: January 1, 2026*  
*Migration Guide Version: 1.0.0*  
*Target Version: v0.8.0-beta*

