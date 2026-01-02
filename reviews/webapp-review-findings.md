# structs-webapp v0.8.0-beta Review Findings

**Date**: January 1, 2026  
**Repository**: [https://github.com/playstructs/structs-webapp/](https://github.com/playstructs/structs-webapp/)  
**Status**: Code Review Complete

---

## Summary

Completed code review of structs-webapp repository. Found evidence of v0.8.0-beta changes in the codebase. Some features are implemented, others may be handled at the database level.

---

## Findings

### ✅ Confirmed: Struct Destroyed Field

**Location**: `src/src/Manager/StructManager.php`

**Evidence**:
- Line 58: `s.is_destroyed = false` in `getAllStructsOnPlanet()` query
- Line 100: `s.is_destroyed = false` in `getStructsByPlayerId()` query

**Status**: ✅ **IMPLEMENTED**
- Struct queries filter out destroyed structs (`is_destroyed = false`)
- Destroyed structs are excluded from API responses
- This aligns with StructSweepDelay behavior (destroyed structs persist for 5 blocks but are filtered from queries)

**Note**: The `is_destroyed` field is used for filtering but may not be explicitly returned in responses. The field exists in the database (per `schemas/database-schema.json`).

### ✅ Confirmed: Raid AttackerRetreated Status

**Location**: `src/js/constants/RaidStatus.js`

**Evidence**:
```javascript
export const RAID_STATUS = {
  REQUESTED: 'requested',
  INITIATED: 'initiated',
  ONGOING: 'ongoing',
  ATTACKER_DEFEATED: 'attackerDefeated',
  ATTACKER_RETREATED: 'attackerRetreated',  // ✅ v0.8.0-beta
  RAID_SUCCESSFUL: 'raidSuccessful',
  DEMILITARIZED: 'demilitarized',
};
```

**Status**: ✅ **IMPLEMENTED**
- `ATTACKER_RETREATED` status is defined in the frontend constants
- Status value: `'attackerRetreated'`

**Note**: The backend queries in `PlanetManager.php` only check for `'initiated'` and `'ongoing'` statuses in active raid queries. The `attackerRetreated` status would be returned when querying completed raids or raid history.

### ⚠️ Partial: Struct Type Cheatsheet Fields

**Location**: `src/src/Manager/StructManager.php`

**Evidence**:
- Line 157-160: `getAllStructTypes()` query uses `SELECT * FROM struct_type`
- This would include all columns from the database table

**Status**: ⚠️ **LIKELY IMPLEMENTED** (Database Level)
- Query selects all columns (`SELECT *`), so `cheatsheet_details` and `cheatsheet_extended_details` would be included if they exist in the database
- According to `schemas/database-schema.json`, these columns were added to `struct_type` table in v0.8.0-beta
- No explicit filtering or selection, so fields should be present in responses

**Recommendation**: Verify by checking actual API response or database schema

### ❌ Not Found: Reactor Endpoints

**Location**: `src/src/Controller/`

**Evidence**:
- No `ReactorController.php` exists
- Reactor data is accessed through:
  - `GuildManager.php` - queries reactor data for guild primary reactor
  - `InfusionManager.php` - queries reactor data for infusions

**Status**: ❌ **NO DEDICATED REACTOR ENDPOINTS**
- No `/api/reactor/{reactor_id}` endpoint
- No `/api/reactor/player/{player_id}` endpoint
- No `/api/reactor/staking/{player_id}` endpoint

**Implication**: Reactor staking information may be:
1. Included in other endpoints (player, guild, infusion)
2. Not exposed via webapp API (only via consensus API)
3. Handled at the frontend level using consensus API directly

### ⚠️ Unknown: Reactor Staking in Player Endpoint

**Location**: `src/src/Manager/PlayerManager.php`

**Status**: ⚠️ **NEEDS VERIFICATION**
- `getPlayer()` query is complex (500+ lines)
- Need to check if reactor staking summary is included
- May need to check the full query or test the endpoint

### ⚠️ Unknown: Reactor Staking in Infusion Endpoint

**Location**: `src/src/Manager/InfusionManager.php`

**Status**: ⚠️ **NEEDS VERIFICATION**
- Infusion queries reference reactor data
- Need to check if staking/delegation information is included in responses

---

## API Endpoints Found

### Controllers Reviewed

1. **PlayerController.php** ✅
   - `GET /api/player/{player_id}` - ✅ Documented
   - `GET /api/player/{player_id}/action/last/block/height` - ✅ Documented
   - `GET /api/player/raid/search` - ✅ Documented
   - `PUT /api/player/username` - ✅ Documented
   - `GET /api/player/transfer/search` - ✅ Documented
   - `GET /api/player/{player_id}/ore/stats` - ✅ Documented
   - `GET /api/player/{player_id}/planet/completed` - ✅ Documented
   - `GET /api/player/{player_id}/raid/launched` - ✅ Documented

2. **StructController.php** ✅
   - `GET /api/struct/player/{player_id}` - ⚠️ **NEW ENDPOINT FOUND** (not in documentation)
   - `GET /api/struct/planet/{planet_id}` - ✅ Documented
   - `GET /api/struct/type` - ✅ Documented
   - `GET /api/struct/{struct_id}` - ✅ Documented

3. **PlanetController.php** ✅
   - `GET /api/planet/{planet_id}` - ✅ Documented
   - `GET /api/planet/{planet_id}/shield/health` - ✅ Documented
   - `GET /api/planet/{planet_id}/shield` - ✅ Documented
   - `GET /api/planet/{planet_id}/raid/active` - ✅ Documented
   - `GET /api/planet/raid/active/fleet/{fleet_id}` - ✅ Documented

4. **InfusionController.php** ✅
   - `GET /api/infusion/player/{player_id}` - ✅ Documented

5. **ReactorController.php** ❌
   - **DOES NOT EXIST** - No reactor-specific endpoints

---

## Documentation Updates Needed

### 1. Add Missing Endpoint

**File**: `api/webapp/struct.yaml`

**Add**:
```yaml
- id: "webapp-struct-by-player"
  method: "GET"
  path: "/api/struct/player/{player_id}"
  description: "Get structs by player ID"
  category: "webapp"
  entity: "Struct"
  parameters:
    - name: "player_id"
      type: "string"
      required: true
      format: "player-id"
```

### 2. Update Struct Endpoint Documentation

**File**: `api/webapp/struct.yaml`

**Update**: Confirm that:
- `is_destroyed` field is filtered (not returned) in struct queries
- Struct type endpoint includes `cheatsheet_details` and `cheatsheet_extended_details` fields

### 3. Update Planet Raid Documentation

**File**: `api/webapp/planet.yaml`

**Update**: Confirm that:
- Raid status includes `attackerRetreated` in the enum
- Active raid queries only return `initiated` and `ongoing` statuses
- Completed raids would include `attackerRetreated` status

### 4. Document Reactor Endpoint Absence

**File**: `protocols/webapp-api-protocol.md`

**Add Note**: 
- No dedicated reactor endpoints exist in webapp API
- Reactor data is accessed via guild and infusion endpoints
- Reactor staking may be handled via consensus API or included in other responses

---

## Verification Status

### ✅ Confirmed
- [x] Struct destroyed field filtering (`is_destroyed = false`)
- [x] Raid attackerRetreated status constant exists
- [x] All documented endpoints exist (except reactor endpoints)

### ⚠️ Needs Verification
- [ ] Struct type cheatsheet fields in API responses
- [ ] Reactor staking information in player endpoint
- [ ] Reactor staking information in infusion endpoint
- [ ] Raid attackerRetreated status in actual API responses

### ❌ Not Found
- [x] Reactor-specific endpoints (confirmed: do not exist)

---

## Next Steps

1. **Test API Endpoints** (if webapp is running)
   - Test struct type endpoint to verify cheatsheet fields
   - Test player endpoint to check for reactor staking data
   - Test infusion endpoint to check for staking/delegation data
   - Test raid endpoints to verify attackerRetreated status

2. **Update Documentation**
   - Add missing `/api/struct/player/{player_id}` endpoint
   - Update struct endpoint notes about destroyed field filtering
   - Update planet raid endpoint notes about attackerRetreated
   - Document absence of reactor endpoints

3. **Verify Database Schema**
   - Check if cheatsheet fields exist in struct_type table
   - Verify is_destroyed column exists in struct table
   - Check reactor-related tables for staking fields

---

## Related Files

- **Review Checklist**: `reviews/webapp-v0.8.0-beta-review.md`
- **Review Workflow**: `reviews/webapp-review-workflow.md`
- **API Documentation**: `api/webapp/`
- **Protocol Documentation**: `protocols/webapp-api-protocol.md`

---

*Review Completed: January 1, 2026*

