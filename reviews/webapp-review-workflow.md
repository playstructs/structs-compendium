# structs-webapp Review Workflow

**Status**: Ready for Code Review  
**Repository**: [https://github.com/playstructs/structs-webapp/](https://github.com/playstructs/structs-webapp/)  
**Date**: January 1, 2026

---

## Overview

This document provides a step-by-step workflow for completing the structs-webapp v0.8.0-beta review. Follow this workflow to systematically review the codebase and update documentation.

---

## Phase 1: Setup and Initial Review

### Step 1: Clone Repository

```bash
# From structs-compendium root directory
git clone https://github.com/playstructs/structs-webapp.git
cd structs-webapp
```

**Verify**: Check that `structs-webapp/` is in `.gitignore` (already done ✅)

### Step 2: Check Git History

```bash
# Check for v0.8.0-beta tag
git tag | grep -E "v0\.8|0\.8\.0"

# Check commits from November-December 2025
git log --oneline --since="2025-11-01" --until="2026-01-01"

# Count commits
git log --oneline --since="2025-11-01" --until="2026-01-01" | wc -l

# See what files changed
git log --oneline --since="2025-11-01" --name-only | grep -E "Controller|Entity|DTO" | sort -u
```

**Document**: Note any significant commits or changes in the review checklist.

### Step 3: Review Project Structure

```bash
# Check Symfony structure
ls -la src/Controller/
ls -la src/Entity/
ls -la migrations/

# Check for API-specific controllers
find src/Controller -name "*Controller.php" | sort
```

**Document**: List all controllers found in the review checklist.

---

## Phase 2: API Endpoint Review

### Step 4: Review Controllers

For each controller file, check:

1. **Route definitions** - Look for `#[Route('/api/...')]` attributes
2. **Method signatures** - Check request/response types
3. **Response structure** - Note what data is returned

**Controllers to Review**:
- [ ] `src/Controller/PlayerController.php`
- [ ] `src/Controller/PlanetController.php`
- [ ] `src/Controller/GuildController.php`
- [ ] `src/Controller/StructController.php`
- [ ] `src/Controller/ReactorController.php` (check if exists)
- [ ] `src/Controller/InfusionController.php`
- [ ] `src/Controller/AuthController.php`
- [ ] `src/Controller/LedgerController.php`

**Command to extract routes**:
```bash
# Find all API routes
grep -r "Route.*'/api" src/Controller/ | grep -v "vendor"
```

**Document**: Update `api/webapp/*.yaml` files with any missing endpoints.

### Step 5: Check for Reactor Endpoints

```bash
# Check if ReactorController exists
ls -la src/Controller/ReactorController.php 2>/dev/null || echo "ReactorController not found"

# Search for reactor routes
grep -r "reactor" src/Controller/ -i | grep -i "route\|api"
```

**Document**: 
- If ReactorController exists, document all endpoints
- If not, note that reactor endpoints may be in PlayerController or elsewhere

### Step 6: Review Entity Changes

For each entity file, check for v0.8.0-beta fields:

**Entities to Review**:
- [ ] `src/Entity/Reactor.php` - Check for staking fields
- [ ] `src/Entity/Struct.php` - Check for `destroyed` field
- [ ] `src/Entity/StructType.php` - Check for cheatsheet fields
- [ ] `src/Entity/Raid.php` - Check for `attackerRetreated` status
- [ ] `src/Entity/Player.php` - Check for reactor staking summary

**Commands**:
```bash
# Check for destroyed field in Struct
grep -n "destroyed" src/Entity/Struct.php

# Check for staking fields in Reactor
grep -n "staking\|delegation" src/Entity/Reactor.php -i

# Check for cheatsheet fields in StructType
grep -n "cheatsheet" src/Entity/StructType.php -i

# Check for attackerRetreated in Raid
grep -n "attackerRetreated\|attacker.*retreat" src/Entity/Raid.php -i
```

**Document**: Update entity schemas if fields are found.

---

## Phase 3: Database and Migration Review

### Step 7: Review Migrations

```bash
# List migrations from November-December 2025
ls -la migrations/ | grep -E "2025-11|2025-12"

# Check migration files for relevant changes
grep -l "destroyed\|cheatsheet\|staking\|permission_hash" migrations/*.php
```

**Document**: Compare with `schemas/database-schema.json` to verify alignment.

### Step 8: Check Database Schema Alignment

**Verify**:
- [ ] `struct` table has `destroyed` column
- [ ] `struct_type` table has `cheatsheet_details` and `cheatsheet_extended_details`
- [ ] Permission system includes `permission_hash`
- [ ] Reactor-related tables include staking fields (if any)

---

## Phase 4: Response Schema Review

### Step 9: Review Response Serialization

```bash
# Check for serialization groups
grep -r "groups.*=" src/Controller/ | grep -E "reactor|staking|destroyed|cheatsheet"

# Check for custom serializers
find src/Serializer -name "*.php" 2>/dev/null
```

**Document**: Note any serialization groups or custom serializers that affect responses.

### Step 10: Test Endpoints (Optional)

If you have the webapp running:

```bash
# Start webapp
cd structs-webapp
docker compose up -d

# Test endpoints
curl http://localhost:8080/api/player/1-11 | jq .
curl http://localhost:8080/api/struct/5-1 | jq .
curl http://localhost:8080/api/planet/2-1/raid/active | jq .

# Check for reactor endpoint
curl http://localhost:8080/api/reactor/3-1 | jq . 2>/dev/null || echo "Reactor endpoint not found"
```

**Document**: Note actual response formats and compare with documentation.

---

## Phase 5: Documentation Updates

### Step 11: Update Endpoint Documentation

For each finding:

1. **New Endpoints**: Add to appropriate `api/webapp/*.yaml` file
2. **Updated Endpoints**: Update existing endpoint documentation
3. **Response Schemas**: Update response examples and schemas
4. **Breaking Changes**: Document any breaking changes

**Files to Update**:
- `api/webapp/player.yaml`
- `api/webapp/planet.yaml`
- `api/webapp/guild.yaml`
- `api/webapp/struct.yaml`
- `api/webapp/infusion.yaml`
- `api/webapp/auth.yaml`
- `api/webapp/reactor.yaml` (create if reactor endpoints exist)
- `protocols/webapp-api-protocol.md`

### Step 12: Update Review Checklist

Mark items as complete in `reviews/webapp-v0.8.0-beta-review.md`:
- [x] Checked for reactor endpoints
- [x] Verified struct destroyed field
- [x] Verified raid attackerRetreated status
- [x] Updated documentation

### Step 13: Create Summary

Update `reviews/webapp-review-summary.md` with:
- Findings from code review
- Endpoints added/updated
- Response schema changes
- Breaking changes (if any)
- Verification status

---

## Phase 6: Verification

### Step 14: Final Verification

- [ ] All endpoints documented
- [ ] Response schemas match actual responses
- [ ] Breaking changes documented
- [ ] Examples updated (if needed)
- [ ] Version numbers updated
- [ ] Review checklist completed

### Step 15: Cleanup (Optional)

```bash
# Remove cloned repository when done
cd ..
rm -rf structs-webapp
```

---

## Quick Reference Commands

### Find All API Routes
```bash
grep -r "Route.*'/api" src/Controller/ | grep -v "vendor" | sort
```

### Find v0.8.0-beta Related Changes
```bash
git log --oneline --since="2025-11-01" --until="2026-01-01" --grep="reactor\|staking\|destroyed\|cheatsheet\|attackerRetreated" -i
```

### Check Entity Properties
```bash
grep -E "private|protected|public" src/Entity/Reactor.php | grep -E "staking|delegation"
grep -E "private|protected|public" src/Entity/Struct.php | grep -i "destroyed"
```

### List All Controllers
```bash
find src/Controller -name "*Controller.php" -exec basename {} \; | sort
```

---

## Review Checklist Status

Track your progress using the checklist in `reviews/webapp-v0.8.0-beta-review.md`.

**Key Areas**:
- [ ] Reactor endpoints found and documented
- [ ] Struct destroyed field verified
- [ ] Raid attackerRetreated status verified
- [ ] Struct type cheatsheet fields verified
- [ ] Infusion endpoint includes staking info
- [ ] Player endpoint includes staking summary
- [ ] All endpoints match documentation

---

## Related Documentation

- **Clone Instructions**: `reviews/webapp-clone-instructions.md`
- **Symfony Review Guide**: `reviews/webapp-symfony-review-guide.md`
- **Review Checklist**: `reviews/webapp-v0.8.0-beta-review.md`
- **Review Summary**: `reviews/webapp-review-summary.md`

---

*Last Updated: January 1, 2026*  
*Status: Ready for Code Review*

