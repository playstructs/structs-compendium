# structs-webapp v0.8.0-beta Review

**Version**: 1.0.0  
**Date**: January 1, 2026  
**Status**: In Progress  
**Priority**: High

---

## Overview

This document tracks the review of `structs-webapp` repository for v0.8.0-beta changes. The webapp is a PHP Symfony application that provides enhanced game state information, player statistics, guild data, and authentication services.

**Repository**: structs-webapp  
**Base URL**: `http://localhost:8080`  
**API Base Path**: `/api`

---

## Review Checklist

### 1. API Endpoint Changes

#### 1.1 New Endpoints (v0.8.0-beta)

- [ ] **Reactor Endpoints**
  - [ ] `GET /api/reactor/{reactor_id}` - Get reactor information (including staking data)
  - [ ] `GET /api/reactor/player/{player_id}` - Get player's reactors with staking info
  - [ ] `GET /api/reactor/staking/{player_id}` - Get player's reactor staking summary
  - [ ] Check if reactor staking delegation status endpoints exist

- [ ] **Infusion Endpoints Updates**
  - [ ] Verify `/api/infusion/player/{player_id}` includes reactor staking information
  - [ ] Check if infusion endpoint response includes delegation status
  - [ ] Verify infusion endpoint includes validation delegation data

- [ ] **Player Endpoints Updates**
  - [ ] Verify `/api/player/{player_id}` includes reactor staking summary
  - [ ] Check if player endpoint includes validation delegation information
  - [ ] Verify player stats include staking-related metrics

- [ ] **Struct Endpoints Updates**
  - [ ] Verify `/api/struct/{struct_id}` includes `destroyed` field (v0.8.0-beta)
  - [ ] Check if struct endpoints handle StructSweepDelay (5 blocks)
  - [ ] Verify struct type endpoints include cheatsheet_details fields

- [ ] **Planet Endpoints Updates**
  - [ ] Verify `/api/planet/{planet_id}/raid/active` includes `attackerRetreated` status
  - [ ] Check if raid endpoints handle new raid outcome status

#### 1.2 Updated Endpoints (v0.8.0-beta)

- [ ] **Authentication Endpoints**
  - [ ] Verify session management unchanged
  - [ ] Check if authentication flow includes permission hash support
  - [ ] Verify Hash permission (bit 64) is handled in auth

- [ ] **Guild Endpoints**
  - [ ] Check if guild endpoints include updated permission information
  - [ ] Verify guild endpoints handle Hash permission

### 2. Authentication & Session Management

- [ ] **Session Management**
  - [ ] Verify session-based authentication unchanged
  - [ ] Check if session expiration handling updated
  - [ ] Verify cookie handling unchanged

- [ ] **Permission System**
  - [ ] Check if Hash permission (bit 64) is supported in auth
  - [ ] Verify permission checking includes Hash permission
  - [ ] Check if permission_hash is exposed in API responses

### 3. Response Schema Changes

#### 3.1 Reactor Staking Fields

- [ ] **Reactor Response Schema**
  - [ ] Verify reactor responses include staking fields:
    - `staking` object with delegation information
    - `delegationStatus` (active, undelegating, migrating)
    - `validationDelegation` information
  - [ ] Check if player-level staking is exposed

#### 3.2 Struct Response Schema

- [ ] **Struct Response Schema**
  - [ ] Verify struct responses include `destroyed` field (boolean)
  - [ ] Check if destroyed structs are filtered or marked appropriately
  - [ ] Verify StructSweepDelay is handled in responses

#### 3.3 Raid Response Schema

- [ ] **Raid Response Schema**
  - [ ] Verify raid responses include `attackerRetreated` status
  - [ ] Check if raid outcome enum includes new status
  - [ ] Verify raid search endpoints handle new status

#### 3.4 Struct Type Response Schema

- [ ] **Struct Type Response Schema**
  - [ ] Verify struct type responses include `cheatsheet_details` field
  - [ ] Check if struct type responses include `cheatsheet_extended_details` field
  - [ ] Verify deprecated charge columns are removed

### 4. Database Integration

- [ ] **Database Schema Alignment**
  - [ ] Verify webapp queries updated struct table (destroyed column)
  - [ ] Check if webapp queries updated struct_type table (cheatsheet fields)
  - [ ] Verify webapp queries updated permission system (permission_hash)
  - [ ] Check if webapp queries updated signer_tx table (Hash permission support)

### 5. Error Handling

- [ ] **Error Responses**
  - [ ] Verify error handling for reactor staking operations
  - [ ] Check if Hash permission errors are properly handled
  - [ ] Verify error messages for new features are clear

### 6. Documentation Updates Needed

#### 6.1 API Documentation Files

- [ ] **`api/webapp/infusion.yaml`**
  - [ ] Update to include reactor staking information
  - [ ] Add v0.8.0-beta notes about validation delegation
  - [ ] Update response schema references

- [ ] **`api/webapp/player.yaml`**
  - [ ] Add reactor staking summary endpoint if exists
  - [ ] Update player response to include staking info
  - [ ] Add v0.8.0-beta notes

- [ ] **`api/webapp/struct.yaml`**
  - [ ] Update struct response to include `destroyed` field
  - [ ] Add v0.8.0-beta notes about StructSweepDelay
  - [ ] Update struct type endpoint to include cheatsheet fields

- [ ] **`api/webapp/planet.yaml`**
  - [ ] Update raid endpoints to include `attackerRetreated` status
  - [ ] Add v0.8.0-beta notes about new raid status

- [ ] **New File: `api/webapp/reactor.yaml`** (if reactor endpoints exist)
  - [ ] Create reactor endpoints documentation
  - [ ] Document reactor staking endpoints
  - [ ] Add examples and response schemas

#### 6.2 Protocol Documentation

- [ ] **`protocols/webapp-api-protocol.md`**
  - [ ] Update version to 1.1.0
  - [ ] Add v0.8.0-beta changes section
  - [ ] Document reactor staking endpoints (if any)
  - [ ] Update authentication section if Hash permission affects auth
  - [ ] Add examples for new features
  - [ ] Update last updated date

#### 6.3 Authentication Documentation

- [ ] **`protocols/authentication.md`**
  - [ ] Check if Hash permission affects authentication flow
  - [ ] Update if session management changed
  - [ ] Add notes about permission checking with Hash permission

### 7. Examples Updates

- [ ] **`examples/auth/webapp-login.json`**
  - [ ] Verify examples are still valid
  - [ ] Update if authentication flow changed

- [ ] **`examples/workflows/authenticated-guild-query.json`**
  - [ ] Verify workflow is still valid
  - [ ] Update if guild endpoints changed

- [ ] **New Examples** (if needed)
  - [ ] Create reactor staking workflow example
  - [ ] Create infusion with staking example
  - [ ] Create struct destroyed query example

---

## v0.8.0-beta Features to Verify

### Feature 1: Hash Permission (Bit 64)

**What to Check**:
- [ ] Hash permission is supported in permission checking
- [ ] Permission hash is exposed in API responses where relevant
- [ ] Authentication flow handles Hash permission correctly

**Expected Impact**:
- Permission checking endpoints may need updates
- Player/guild permission responses may include Hash permission

### Feature 2: Reactor Staking & Validation Delegation

**What to Check**:
- [ ] Reactor endpoints exist or are needed
- [ ] Infusion endpoint includes staking information
- [ ] Player endpoint includes reactor staking summary
- [ ] Delegation statuses (active, undelegating, migrating) are exposed

**Expected Impact**:
- New reactor endpoints may be needed
- Infusion endpoint response schema may need updates
- Player endpoint response may need staking summary

### Feature 3: New Raid Status (attackerRetreated)

**What to Check**:
- [ ] Raid endpoints include `attackerRetreated` in status enum
- [ ] Raid search endpoints handle new status
- [ ] Raid response schemas include new status

**Expected Impact**:
- Planet raid endpoints need status enum updates
- Raid search endpoints may need filtering updates

### Feature 4: StructSweepDelay & Destroyed Field

**What to Check**:
- [ ] Struct endpoints include `destroyed` field in responses
- [ ] Destroyed structs are handled appropriately (filtered or marked)
- [ ] Struct type endpoints include cheatsheet fields

**Expected Impact**:
- Struct endpoint response schemas need `destroyed` field
- Struct type endpoint response schemas need cheatsheet fields

---

## Repository Access

**Repository URL**: [https://github.com/playstructs/structs-webapp/](https://github.com/playstructs/structs-webapp/)

**Technology Stack**:
- PHP 8.2
- Symfony 6.3 Framework
- Doctrine ORM
- Apache web server

**Note**: Direct access to structs-webapp repository is needed to complete this review.

### Steps to Complete Review

1. **Clone Repository**
   ```bash
   # Clone into workspace root (structs-webapp/ is in .gitignore)
   git clone https://github.com/playstructs/structs-webapp.git
   cd structs-webapp
   ```
   
   **Note**: The `structs-webapp/` directory is excluded from git via `.gitignore` to prevent committing the cloned repository.

2. **Check Git History for v0.8.0-beta**
   ```bash
   git tag | grep v0.8.0-beta
   git log --oneline --grep="v0.8.0-beta"
   git log --oneline --since="2025-11-01" --until="2026-01-01"
   ```

3. **Review Symfony API Controllers**
   - Location: `src/Controller/` or `src/Controller/Api/`
   - Look for controller files:
     - `PlayerController.php` - Player endpoints
     - `PlanetController.php` - Planet endpoints
     - `GuildController.php` - Guild endpoints
     - `StructController.php` - Struct endpoints
     - `ReactorController.php` - Reactor endpoints (may be new)
     - `InfusionController.php` - Infusion endpoints
     - `AuthController.php` - Authentication endpoints
     - `LedgerController.php` - Ledger endpoints

4. **Review Routes Configuration**
   - Check `config/routes.yaml` or `config/routes/`
   - Look for API route definitions
   - Check for new routes added in v0.8.0-beta timeframe

5. **Review Response DTOs/Schemas**
   - Location: `src/DTO/` or `src/Entity/`
   - Check for:
     - Reactor staking fields in Reactor entity/DTO
     - `destroyed` field in Struct entity/DTO
     - `cheatsheet_details` in StructType entity/DTO
     - `attackerRetreated` in Raid entity/DTO
     - Permission hash fields

6. **Review Database Schema Changes**
   - Check Doctrine migrations: `migrations/`
   - Look for migrations dated November-December 2025
   - Verify alignment with `schemas/database-schema.json`

7. **Review API Response Examples**
   - Check controller methods for response structure
   - Look for serialization groups or DTOs
   - Compare with documented response schemas

8. **Test Endpoints** (if possible)
   - Start webapp: `docker compose up -d`
   - Test endpoints: `curl http://localhost:8080/api/...`
   - Verify response formats match documentation

9. **Update Documentation**
   - Add any missing endpoints
   - Update response schemas
   - Document breaking changes
   - Add examples for new features

---

## Findings Summary

### Current Status

**Documentation Last Updated**: January 2025  
**Documentation Version**: 1.0.0  
**Expected Updates Needed**: Based on v0.8.0-beta features

### Identified Gaps

1. **Reactor Endpoints**: No reactor-specific endpoints documented (may need to be added)
2. **Infusion Endpoint**: May need updates for reactor staking information
3. **Struct Endpoint**: Needs `destroyed` field documentation
4. **Planet Raid Endpoint**: Needs `attackerRetreated` status documentation
5. **Struct Type Endpoint**: Needs cheatsheet fields documentation

### Next Steps

1. ✅ Review current documentation state
2. ⏳ Access structs-webapp repository
3. ⏳ Compare code changes with documentation
4. ⏳ Update API endpoint documentation
5. ⏳ Update protocol documentation
6. ⏳ Update examples if needed
7. ⏳ Verify all changes are documented

---

## Related Documentation

- **Review Workflow**: `reviews/webapp-review-workflow.md` - Step-by-step review workflow
- **Clone Instructions**: `reviews/webapp-clone-instructions.md` - How to clone and set up the repository
- **Review Guide**: `reviews/webapp-symfony-review-guide.md` - Symfony code review guide
- **Review Summary**: `reviews/webapp-review-summary.md` - Review summary document
- **CHANGELOG.md** - v0.8.0-beta changes summary
- **protocols/webapp-api-protocol.md** - Main webapp API protocol
- **api/webapp/** - Webapp endpoint definitions
- **schemas/entities/reactor.json** - Reactor entity schema (includes staking)
- **schemas/entities/struct.json** - Struct entity schema (includes destroyed field)
- **schemas/gameplay.json** - Gameplay schemas (includes attackerRetreated)

---

## Symfony-Specific Review Areas

### Controller Structure

In Symfony 6.3, API controllers are typically located in:
- `src/Controller/Api/` - API-specific controllers
- `src/Controller/` - General controllers

Look for:
- `#[Route('/api/...')]` attributes on controller classes
- `#[Route('/api/...', methods: ['GET', 'POST', ...])]` on controller methods
- Response serialization (Symfony Serializer, JMS Serializer, or custom)

### Key Files to Review

1. **Controllers** (`src/Controller/` or `src/Controller/Api/`)
   - `PlayerController.php` - `/api/player/*` endpoints
   - `PlanetController.php` - `/api/planet/*` endpoints
   - `GuildController.php` - `/api/guild/*` endpoints
   - `StructController.php` - `/api/struct/*` endpoints
   - `ReactorController.php` - `/api/reactor/*` endpoints (check if exists)
   - `InfusionController.php` - `/api/infusion/*` endpoints
   - `AuthController.php` - `/api/auth/*` endpoints
   - `LedgerController.php` - `/api/ledger/*` endpoints

2. **Entities/DTOs** (`src/Entity/` or `src/DTO/`)
   - `Player.php` - Player entity with reactor staking fields
   - `Reactor.php` - Reactor entity with staking information
   - `Struct.php` - Struct entity with `destroyed` field
   - `StructType.php` - StructType entity with cheatsheet fields
   - `Raid.php` - Raid entity with `attackerRetreated` status

3. **Routes** (`config/routes.yaml` or `config/routes/`)
   - API route definitions
   - Route annotations/attributes

4. **Serialization** (`config/serializer.yaml` or `src/Serializer/`)
   - Response serialization configuration
   - Custom serializers for new fields

5. **Migrations** (`migrations/`)
   - Database schema changes
   - Look for migrations dated November-December 2025

### What to Look For

#### Reactor Staking Endpoints
```php
// Look for in ReactorController.php or PlayerController.php
#[Route('/api/reactor/{id}', methods: ['GET'])]
public function getReactor(int $id): JsonResponse
{
    // Check if response includes staking fields
}

#[Route('/api/reactor/player/{playerId}', methods: ['GET'])]
public function getPlayerReactors(int $playerId): JsonResponse
{
    // Check if response includes staking information
}
```

#### Struct Destroyed Field
```php
// Look for in StructController.php
#[Route('/api/struct/{id}', methods: ['GET'])]
public function getStruct(int $id): JsonResponse
{
    // Check if Struct entity has 'destroyed' property
    // Check if response includes destroyed field
}
```

#### Raid AttackerRetreated Status
```php
// Look for in PlanetController.php
#[Route('/api/planet/{id}/raid/active', methods: ['GET'])]
public function getActiveRaid(int $id): JsonResponse
{
    // Check if Raid entity has 'attackerRetreated' status
    // Check if response includes new status
}
```

---

*Review Status: In Progress - Repository Access Needed*  
*Repository URL: https://github.com/playstructs/structs-webapp/*  
*Last Updated: January 1, 2026*  
*Next Review: After repository access and code review*

