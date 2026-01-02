# structs-webapp Symfony Code Review Guide

**Repository**: [https://github.com/playstructs/structs-webapp/](https://github.com/playstructs/structs-webapp/)  
**Framework**: Symfony 6.3  
**Language**: PHP 8.2  
**Purpose**: Guide for reviewing API endpoints and v0.8.0-beta changes

---

## Quick Start

### Clone Repository

**Note**: The `structs-webapp/` directory is in `.gitignore` and will not be committed to this repository.

**See**: `reviews/webapp-clone-instructions.md` for detailed cloning instructions.

```bash
# From structs-compendium root directory
# Clone into the workspace root (will be ignored by git)
git clone https://github.com/playstructs/structs-webapp.git
cd structs-webapp
```

### Check for v0.8.0-beta Changes
```bash
# Check for tags
git tag | grep -E "v0\.8|0\.8\.0"

# Check commit history (November-December 2025)
git log --oneline --since="2025-11-01" --until="2026-01-01"

# Check specific files changed
git log --oneline --since="2025-11-01" --name-only | grep -E "Controller|Entity|DTO"
```

---

## Symfony Project Structure

### Key Directories

```
structs-webapp/
├── src/
│   ├── Controller/          # API controllers (main location)
│   │   ├── Api/             # API-specific controllers (if exists)
│   │   ├── PlayerController.php
│   │   ├── PlanetController.php
│   │   ├── GuildController.php
│   │   ├── StructController.php
│   │   ├── ReactorController.php  # Check if exists (v0.8.0-beta)
│   │   ├── InfusionController.php
│   │   ├── AuthController.php
│   │   └── LedgerController.php
│   ├── Entity/              # Doctrine entities
│   │   ├── Player.php
│   │   ├── Reactor.php
│   │   ├── Struct.php
│   │   ├── StructType.php
│   │   └── Raid.php
│   ├── DTO/                 # Data Transfer Objects (if used)
│   └── Serializer/          # Custom serializers (if used)
├── config/
│   ├── routes.yaml          # Route definitions
│   ├── routes/              # Route files (if split)
│   └── serializer.yaml      # Serialization config
└── migrations/              # Doctrine migrations
```

---

## Review Checklist

### 1. Check for Reactor Endpoints

**Location**: `src/Controller/ReactorController.php` or `src/Controller/PlayerController.php`

**Look for**:
```php
#[Route('/api/reactor/{id}', methods: ['GET'])]
public function getReactor(int $id): JsonResponse

#[Route('/api/reactor/player/{playerId}', methods: ['GET'])]
public function getPlayerReactors(int $playerId): JsonResponse

#[Route('/api/reactor/staking/{playerId}', methods: ['GET'])]
public function getReactorStaking(int $playerId): JsonResponse
```

**What to verify**:
- Response includes staking fields
- Response includes delegation status (active, undelegating, migrating)
- Response includes validation delegation information

### 2. Check Infusion Endpoint Updates

**Location**: `src/Controller/InfusionController.php`

**Look for**:
```php
#[Route('/api/infusion/player/{playerId}', methods: ['GET'])]
public function getPlayerInfusions(int $playerId): JsonResponse
```

**What to verify**:
- Response includes reactor staking information
- Response includes validation delegation status
- Response structure matches v0.8.0-beta requirements

### 3. Check Player Endpoint Updates

**Location**: `src/Controller/PlayerController.php`

**Look for**:
```php
#[Route('/api/player/{id}', methods: ['GET'])]
public function getPlayer(int $id): JsonResponse
```

**What to verify**:
- Response includes reactor staking summary
- Response includes validation delegation information
- Response includes staking-related metrics

### 4. Check Struct Endpoint Updates

**Location**: `src/Controller/StructController.php`

**Look for**:
```php
#[Route('/api/struct/{id}', methods: ['GET'])]
public function getStruct(int $id): JsonResponse

#[Route('/api/struct/type', methods: ['GET'])]
public function getStructTypes(): JsonResponse
```

**What to verify**:
- Struct response includes `destroyed` field (boolean)
- Struct type response includes `cheatsheet_details` field
- Struct type response includes `cheatsheet_extended_details` field

### 5. Check Planet Raid Endpoint Updates

**Location**: `src/Controller/PlanetController.php`

**Look for**:
```php
#[Route('/api/planet/{id}/raid/active', methods: ['GET'])]
public function getActiveRaid(int $id): JsonResponse
```

**What to verify**:
- Response includes `attackerRetreated` in status enum
- Response includes new raid outcome status
- Raid search endpoints handle new status

### 6. Check Entity/DTO Updates

**Location**: `src/Entity/` or `src/DTO/`

**Files to check**:
- `Reactor.php` - Check for staking fields
- `Struct.php` - Check for `destroyed` property
- `StructType.php` - Check for `cheatsheet_details` and `cheatsheet_extended_details`
- `Raid.php` - Check for `attackerRetreated` status
- `Player.php` - Check for reactor staking summary fields

**Example checks**:
```php
// In Reactor.php
private ?array $staking = null;
private ?string $delegationStatus = null;

// In Struct.php
private bool $destroyed = false;

// In StructType.php
private ?string $cheatsheetDetails = null;
private ?string $cheatsheetExtendedDetails = null;

// In Raid.php
private ?string $status = null; // Check if includes 'attackerRetreated'
```

### 7. Check Database Migrations

**Location**: `migrations/`

**Look for**:
- Migrations dated November-December 2025
- Changes to `struct` table (destroyed column)
- Changes to `struct_type` table (cheatsheet columns)
- Changes to `reactor` table (staking fields)
- Changes to `raid` table (attackerRetreated status)

**Command**:
```bash
ls -la migrations/ | grep -E "2025-11|2025-12"
```

### 8. Check Routes Configuration

**Location**: `config/routes.yaml` or `config/routes/`

**Look for**:
- New API routes added in v0.8.0-beta timeframe
- Route changes for existing endpoints
- Route annotations/attributes

---

## Common Symfony Patterns

### Route Definition (Symfony 6.3)

```php
// Using attributes (modern Symfony)
#[Route('/api/player/{id}', name: 'api_player_get', methods: ['GET'])]
public function getPlayer(int $id): JsonResponse
{
    // ...
}
```

### Response Serialization

```php
// Using Symfony Serializer
return $this->json($player, Response::HTTP_OK, [], [
    'groups' => ['player:read', 'reactor:read'] // Check for new groups
]);
```

### Entity Properties

```php
// Check for new properties in entities
#[ORM\Column(type: 'boolean', nullable: true)]
private ?bool $destroyed = null;

#[ORM\Column(type: 'json', nullable: true)]
private ?array $staking = null;
```

---

## v0.8.0-beta Specific Checks

### Reactor Staking

**Check**:
1. Does `ReactorController.php` exist?
2. Does `Reactor` entity have staking fields?
3. Do responses include delegation status?
4. Is player-level staking exposed?

**Files to review**:
- `src/Controller/ReactorController.php` (if exists)
- `src/Entity/Reactor.php`
- `src/Entity/Player.php` (for staking summary)

### Hash Permission

**Check**:
1. Does authentication handle Hash permission (bit 64)?
2. Are permission responses updated?
3. Is permission_hash exposed in API?

**Files to review**:
- `src/Controller/AuthController.php`
- `src/Entity/Player.php` (permission fields)
- `src/Entity/Guild.php` (permission fields)

### Struct Destroyed Field

**Check**:
1. Does `Struct` entity have `destroyed` property?
2. Do struct endpoints include `destroyed` in response?
3. Are destroyed structs filtered appropriately?

**Files to review**:
- `src/Entity/Struct.php`
- `src/Controller/StructController.php`

### Raid AttackerRetreated

**Check**:
1. Does `Raid` entity include `attackerRetreated` status?
2. Do raid endpoints include new status?
3. Are raid search endpoints updated?

**Files to review**:
- `src/Entity/Raid.php`
- `src/Controller/PlanetController.php`

---

## Testing Endpoints

### Start Webapp
```bash
docker compose up -d
```

### Test Endpoints
```bash
# Test player endpoint
curl http://localhost:8080/api/player/1-11

# Test reactor endpoint (if exists)
curl http://localhost:8080/api/reactor/3-1

# Test struct endpoint
curl http://localhost:8080/api/struct/5-1

# Test planet raid endpoint
curl http://localhost:8080/api/planet/2-1/raid/active
```

### Check Response Formats
- Verify JSON structure matches documentation
- Check for new fields (staking, destroyed, etc.)
- Verify status codes and error handling

---

## Documentation Update Checklist

After reviewing code:

- [ ] Add any missing endpoints to `api/webapp/*.yaml` files
- [ ] Update response schemas in endpoint files
- [ ] Update `protocols/webapp-api-protocol.md` with findings
- [ ] Add examples for new endpoints
- [ ] Update version numbers if needed
- [ ] Document breaking changes (if any)
- [ ] Update review checklist with findings

---

## Related Files

- **Review Checklist**: `reviews/webapp-v0.8.0-beta-review.md`
- **Review Summary**: `reviews/webapp-review-summary.md`
- **API Documentation**: `api/webapp/`
- **Protocol Documentation**: `protocols/webapp-api-protocol.md`
- **Entity Schemas**: `schemas/entities/`

---

*Last Updated: January 1, 2026*  
*Repository: https://github.com/playstructs/structs-webapp/*

