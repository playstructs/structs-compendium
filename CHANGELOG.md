# Changelog

All notable changes to the Structs Compendium documentation will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-01-01

### Added - Permissions

- **Hash Permission** (v0.8.0-beta)
  - Added new Hash permission bit to permission system
  - Permission values are bit-based flags that can be combined
  - Hash permission bit value: 64
  - Updated `schemas/game-state.json` with permission bit documentation
  - See `schemas/game-state.json#/definitions/Permission` for details

### Added - Combat & Raids

- **New Raid Status: `attackerRetreated`** (v0.8.0-beta)
  - Added `attackerRetreated` status to raid outcome enum in `schemas/gameplay.json`
  - Updated `api/streaming/event-schemas.json` to include attackerRetreated in PlanetRaidStatusEvent
  - Documented new status in `protocols/gameplay-protocol.md`
  - Updated `examples/gameplay-combat-bot.json` to handle attackerRetreated status
  - Status indicates attacker retreated from raid before completion
  - No resources gained or lost when attacker retreats

### Added - Reactor Staking & Validation Delegation

- **Reactor Staking Functionality** (v0.8.0-beta)
  - Added reactor staking fields to `schemas/entities/reactor.json`
  - Documented player-level staking management
  - Added staking delegation statuses: `active`, `undelegating`, `migrating`
  - Updated `schemas/economics.json` with reactor staking section
  - Updated `reference/entity-index.json` with staking information

- **Validation Delegation Actions** (v0.8.0-beta)
  - Updated `MsgReactorInfuse` to handle validation delegation
  - Updated `MsgReactorDefuse` to handle validation undelegation
  - Added `MsgReactorBeginMigration` action for redelegation
  - Added `MsgReactorCancelDefusion` action to cancel undelegation
  - All actions documented in `schemas/actions.json`
  - Added comprehensive workflow examples in `protocols/economic-protocol.md`
  - Added reactor staking examples to `examples/economic-bot.json`

### Added - Struct Lifecycle

- **StructSweepDelay** (v0.8.0-beta)
  - Added block-based delay (5 blocks) for struct sweeping
  - Documented in `lifecycles/struct-lifecycle.md`
  - Added constant to `schemas/gameplay.json`
  - Planet/fleet slot references persist during delay period
  - Slots fully cleared after 5 blocks

### Changed - Fleet Movement

- **Fleet Movement Improvements** (v0.8.0-beta)
  - Updated `lifecycles/fleet-lifecycle.md` with bug fix documentation
  - Improved fleet movement logic and validation
  - Better error handling for invalid movements
  - More reliable fleet status transitions

### Changed - Membership Join Process

- **Streamlined Redelegation** (v0.8.0-beta)
  - Updated `protocols/economic-protocol.md` with membership join improvements
  - Staked assets automatically redelegated during migration
  - Streamlined process for joining guilds

### Fixed - Bug Resolutions

- **GetInfusionByID Array Split Bug** (v0.8.0-beta)
  - Bug in GetInfusionByID array split logic resolved
  - Documented in `schemas/errors.json` and `troubleshooting/common-issues.md`

- **EventGuildBankAddress Format Bug** (v0.8.0-beta)
  - Bug in EventGuildBankAddress format resolved
  - Documented in `schemas/errors.json` and `troubleshooting/common-issues.md`

- **BankAddress Format Logging Bug** (v0.8.0-beta)
  - Bug in BankAddress format logging resolved
  - Documented in `schemas/errors.json` and `troubleshooting/common-issues.md`

### Updated - Documentation

- **Version Updates**
  - Updated version numbers to 1.1.0 in all major schema files
  - Updated `AGENTS.md` version and last updated date
  - Updated `DOCUMENTATION_INDEX.md` version and last updated date
  - Updated `reference/action-quick-reference.md` with new actions

- **Quick References**
  - Updated `reference/action-quick-reference.md` with reactor staking actions
  - Added documentation for new validation delegation actions
  - Updated combat/raid quick reference with attackerRetreated status
  - Updated `reference/entity-index.json` with Hash permission information

- **Examples**
  - Updated `examples/gameplay-combat-bot.json` with attackerRetreated handling
  - Added reactor staking workflow to `examples/economic-bot.json`
  - Updated version to 1.1.0 in economic bot example

- **Error Handling**
  - Added resolved bugs section to `troubleshooting/common-issues.md`
  - Updated `schemas/errors.json` with v0.8.0-beta resolved bugs metadata
  - Documented all resolved bugs from v0.8.0-beta

### Added - Database Schema

- **Database Schema Documentation** (v0.8.0-beta)
  - Created `schemas/database-schema.json` documenting PostgreSQL schema changes
  - Documented all structs-pg repository changes from November-December 2025
  - See `schemas/database-schema.json` for complete database change log

- **Struct Table Changes** (v0.8.0-beta)
  - Added `destroyed` column to struct table (2025-12-29)
  - Tracks struct destruction status
  - Related to StructSweepDelay (5 blocks) - destroyed structs persist for 5 blocks
  - Updated `schemas/entities/struct.json` with destroyed field

- **Struct Type Table Changes** (v0.8.0-beta)
  - Added `cheatsheet_details` column (2025-11-29)
  - Added `cheatsheet_extended_details` column (2025-12-02)
  - Removed deprecated charge columns for mine and refine (2025-12-08)
  - Updated `schemas/entities/struct-type.json` with database change metadata

- **Permission System Database Changes** (v0.8.0-beta)
  - Added `permission_hash` level to permission view (2025-12-18)
  - Updated signer_tx table to support Hash permission (2025-12-18)
  - Added permissions column to player_address_pending table (2025-12-29)
  - Maps to Hash permission bit (value 64) in API layer

- **Transaction Signing Changes** (v0.8.0-beta)
  - Fixed Hash Complete nonces: INTEGER → CHARACTER VARYING (2025-12-15)
  - Updated transaction signing permission levels (2025-12-18)
  - Added new transaction types (2025-12-18)
  - Fixed signer account bug (2025-12-29)

- **View Improvements** (v0.8.0-beta)
  - Fixed raid logic in view.work (2025-12-13)
  - Improved exclusion logic to exclude impossible tasks (2025-12-27)
  - Removed deprecated charge columns from views (2025-12-08)

- **Cache and Trigger Updates** (v0.8.0-beta)
  - Fixed struct type cache commit (2025-12-18)
  - Fixed GRASS grid p values (2025-12-19)
  - Updated cache triggers for deprecated column removal (2025-12-08)

### Technical Details

- **Schema Changes**
  - `schemas/game-state.json`: Added Hash permission bit (64) documentation
  - `schemas/gameplay.json`: Added attackerRetreated status, StructSweepDelay constant
  - `schemas/entities/reactor.json`: Added staking fields and delegation properties
  - `schemas/entities.json`: Updated Permission entity with Hash permission info
  - `schemas/entities/struct.json`: Added destroyed field, updated version to 1.1.0
  - `schemas/entities/struct-type.json`: Added database change metadata, updated version to 1.1.0
  - `schemas/actions.json`: Updated Reactor Infuse/Defuse, added Begin Migration/Cancel Defusion
  - `schemas/economics.json`: Added reactor staking section
  - `schemas/errors.json`: Added v0.8.0-beta resolved bugs metadata
  - `schemas/database-schema.json`: New file documenting PostgreSQL schema changes

- **Protocol Updates**
  - `protocols/gameplay-protocol.md`: Added attackerRetreated status documentation
  - `protocols/economic-protocol.md`: Added reactor staking and validation delegation sections

- **Lifecycle Updates**
  - `lifecycles/struct-lifecycle.md`: Documented StructSweepDelay
  - `lifecycles/fleet-lifecycle.md`: Documented fleet movement improvements

---

## [1.0.0] - 2025-12-07

### Initial Release

- Comprehensive AI agent documentation
- Complete schema definitions
- Protocol documentation
- Example implementations
- Quick reference guides

---

## Version History

- **1.1.0** (2026-01-01): v0.8.0-beta updates - Hash permission, reactor staking, attackerRetreated status, struct sweep delay, database schema changes, bug fixes
- **1.0.0** (2025-12-07): Initial release

---

*For detailed information about each change, see the relevant documentation files.*

