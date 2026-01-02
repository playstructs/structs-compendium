# structs-mcp v0.8.0-beta Review Findings

**Date**: January 1, 2026  
**Repository**: [https://github.com/playstructs/structs-mcp/](https://github.com/playstructs/structs-mcp/)  
**Status**: Code Review Complete

---

## Summary

Completed code review of structs-mcp repository. The MCP server provides tools for AI agents to interact with Structs. Found that reactor query tools exist, but reactor staking action tools may need to be added or verified.

---

## Findings

### ✅ Confirmed: Reactor Query Tools

**Location**: `src/tools/query.ts`, `src/tools/definitions/query-tools.ts`

**Evidence**:
- `structs_query_reactor` tool exists (line 28 in query-tools.ts)
- `queryReactor()` function implemented (lines 995-1042 in query.ts)
- Reactor entity type supported (type 3)

**Status**: ✅ **IMPLEMENTED**
- Reactor query tool available: `structs_query_reactor`
- Queries consensus API: `/structs/reactor/{reactorId}`
- Returns reactor data including owner and power generation

**Note**: The reactor query tool queries the consensus API, which should include staking fields if they exist in v0.8.0-beta.

### ❌ Not Found: Reactor Staking Action Tools

**Location**: `src/tools/action.ts`, `src/tools/definitions/action-tools.ts`

**Evidence**:
- No dedicated reactor infuse/defuse action tools found
- `structs_action_submit_transaction` functionMap (lines 41-52) does NOT include reactor staking actions
- Supported actions: `explore`, `struct_build_initiate`, `struct_build_complete`, `planet_raid_complete`, `ore_miner_complete`, `ore_refinery_complete`, `fleet_move`, `guild_membership_join`, `guild_membership_leave`
- Switch statement (lines 67-143) does NOT handle reactor staking actions

**Status**: ❌ **NOT IMPLEMENTED**
- Reactor staking actions are NOT supported in `structs_action_submit_transaction`
- Missing action types:
  - `reactor-infuse` ❌
  - `reactor-defuse` ❌
  - `reactor-begin-migration` ❌
  - `reactor-cancel-defusion` ❌

**Recommendation**: Add reactor staking action support to `structs_action_submit_transaction` by:
1. Adding to functionMap: `'reactor-infuse': 'tx_reactor_infuse'`, etc.
2. Adding switch cases for each reactor action
3. Updating action enum in `action-tools.ts` to include reactor actions

### ✅ Confirmed: Query Tools Support All Entities

**Location**: `src/tools/definitions/query-tools.ts`

**Evidence**:
- Query tools exist for all entity types including Reactor
- List tools exist for structs, struct_types, etc.
- Query tools use factory functions for consistency

**Status**: ✅ **IMPLEMENTED**
- All entity types have query tools
- Struct query tools should return destroyed field if present
- Struct type query tools should return cheatsheet fields if present

### ⚠️ Unknown: Struct Destroyed Field Support

**Location**: `src/tools/query.ts`

**Status**: ⚠️ **LIKELY SUPPORTED** (Pass-through)
- Struct query tools query consensus API directly
- If consensus API includes `destroyed` field, it will be returned
- No explicit filtering found (unlike webapp which filters destroyed structs)

### ⚠️ Unknown: Raid AttackerRetreated Status

**Location**: `src/tools/query.ts`

**Status**: ⚠️ **LIKELY SUPPORTED** (Pass-through)
- Raid queries go through consensus API
- If consensus API includes `attackerRetreated` status, it will be returned
- No explicit status filtering found

---

## MCP Tools Found

### Action Tools

1. **`structs_action_submit_transaction`** ✅
   - Supports: `explore`, `struct_build_initiate`, `struct_build_complete`, `fleet_move`, `guild_membership_join`, `guild_membership_leave`
   - ⚠️ **Unknown**: Does it support reactor staking actions?

2. **`structs_action_create_player`** ✅
   - Creates new player accounts

3. **`structs_action_build_struct`** ✅
   - Builds struct from start to finish

4. **`structs_action_activate_struct`** ✅
   - Activates a struct

5. **`structs_action_attack`** ✅
   - Attacks another struct

6. **`structs_action_move_fleet`** ✅
   - Moves a fleet

### Query Tools

1. **`structs_query_reactor`** ✅
   - Queries reactor information
   - Should include staking data if present in API

2. **`structs_query_struct`** ✅
   - Queries struct information
   - Should include destroyed field if present

3. **`structs_query_planet`** ✅
   - Queries planet information
   - Should include raid status with attackerRetreated if present

4. **List Tools** ✅
   - `structs_list_structs`
   - `structs_list_struct_types` (should include cheatsheet fields)

### Calculation Tools

1. **`structs_calculate_power`** ✅
   - Calculates power from Alpha Matter
   - Supports reactor generator type

---

## v0.8.0-beta Features Status

### Reactor Staking

**Query Support**: ✅
- Reactor query tool exists and should return staking data from consensus API

**Action Support**: ❌ **NOT IMPLEMENTED**
- `structs_action_submit_transaction` does NOT support reactor staking actions
- Missing actions:
  - `reactor-infuse` ❌
  - `reactor-defuse` ❌
  - `reactor-begin-migration` ❌
  - `reactor-cancel-defusion` ❌

### Hash Permission

**Status**: ⚠️ **UNKNOWN**
- Permission checking tools exist but need to verify Hash permission (bit 64) support
- May be handled at validation level

### Struct Destroyed Field

**Status**: ✅ **LIKELY SUPPORTED**
- Struct query tools pass through all fields from consensus API
- If consensus API includes `destroyed` field, it will be returned

### Raid AttackerRetreated

**Status**: ✅ **LIKELY SUPPORTED**
- Raid queries pass through all statuses from consensus API
- If consensus API includes `attackerRetreated` status, it will be returned

---

## Documentation Updates Needed

### 1. Verify Reactor Staking Actions

**Action**: Check if `structs_action_submit_transaction` supports reactor staking action types, or document that they need to be added.

**Files to Check**:
- `src/tools/action.ts` - Check action type enum
- `src/tools/handlers/action-handlers.ts` - Check handler support

### 2. Update Tool Documentation

**If reactor staking actions are supported**:
- Document reactor-infuse, reactor-defuse, reactor-begin-migration, reactor-cancel-defusion action types
- Add examples for reactor staking workflows

**If reactor staking actions are NOT supported**:
- Document that reactor staking actions need to be added
- Note that reactor queries return staking data but actions are not yet available

### 3. Verify Query Tool Responses

**Action**: Document that query tools return all fields from consensus API, including:
- Reactor staking fields (if present)
- Struct destroyed field (if present)
- Raid attackerRetreated status (if present)
- Struct type cheatsheet fields (if present)

---

## Recommendations

### High Priority

1. **Add Reactor Staking Actions** ✅ **IDENTIFIED GAP**
   - `structs_action_submit_transaction` does NOT support reactor staking actions
   - **Required Changes**:
     - Add to `functionMap` in `src/tools/action.ts`:
       - `'reactor-infuse': 'tx_reactor_infuse'`
       - `'reactor-defuse': 'tx_reactor_defuse'`
       - `'reactor-begin-migration': 'tx_reactor_begin_migration'`
       - `'reactor-cancel-defusion': 'tx_reactor_cancel_defusion'`
     - Add switch cases for each reactor action
     - Update action enum in `src/tools/definitions/action-tools.ts` to include reactor actions
     - Verify database functions exist: `signer.tx_reactor_*`

2. **Document Missing Features**
   - Document that reactor staking actions are not yet implemented
   - Note that reactor queries work but actions are missing
   - Add to roadmap/backlog

### Medium Priority

3. **Document Query Tool Responses**
   - Document that query tools return all fields from consensus API
   - Note that v0.8.0-beta fields (staking, destroyed, attackerRetreated, cheatsheet) are included if present

4. **Add Examples**
   - Add reactor staking workflow examples
   - Add examples showing reactor query with staking data

---

## Verification Status

### ✅ Confirmed
- [x] Reactor query tool exists
- [x] All entity query tools exist
- [x] Struct query tools exist
- [x] Planet query tools exist

### ⚠️ Needs Verification
- [ ] Reactor query returns staking data (likely - passes through from API)
- [ ] Struct query returns destroyed field (likely - passes through from API)
- [ ] Raid query returns attackerRetreated status (likely - passes through from API)
- [ ] Struct type query returns cheatsheet fields (likely - passes through from API)
- [ ] Hash permission support in validation tools

### ❌ Confirmed Missing
- [x] Reactor staking action support in `structs_action_submit_transaction` (confirmed: NOT implemented)

### ❌ Not Found
- [x] Dedicated reactor staking action tools (confirmed: do not exist as separate tools)

---

## Next Steps

1. **Add Reactor Staking Actions** (High Priority)
   - Update `src/tools/action.ts`:
     - Add reactor actions to `functionMap`
     - Add switch cases for reactor actions
     - Verify database function names match (`signer.tx_reactor_*`)
   - Update `src/tools/definitions/action-tools.ts`:
     - Add reactor actions to action enum
     - Update tool description to include reactor actions

2. **Test MCP Tools** (if MCP server is running)
   - Test `structs_query_reactor` to verify staking data
   - Test struct queries to verify destroyed field
   - Test raid queries to verify attackerRetreated status
   - Test struct type queries to verify cheatsheet fields

3. **Update Documentation**
   - Document that reactor staking actions need to be added
   - Update tool descriptions when actions are added
   - Add examples for reactor staking workflows
   - Note that query tools pass through all API fields

---

## Related Files

- **Review Checklist**: `reviews/mcp-v0.8.0-beta-review.md`
- **Action Definitions**: `structs-mcp/src/tools/definitions/action-tools.ts`
- **Query Definitions**: `structs-mcp/src/tools/definitions/query-tools.ts`
- **Action Handlers**: `structs-mcp/src/tools/handlers/action-handlers.ts`
- **Action Implementation**: `structs-mcp/src/tools/action.ts`

---

*Review Completed: January 1, 2026*

