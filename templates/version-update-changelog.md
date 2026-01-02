# CHANGELOG Update Template

**Version**: Template  
**Last Updated**: [Current Date]  
**Purpose**: Template for updating CHANGELOG.md with new version information

---

## Usage

When creating a new version entry in CHANGELOG.md, use this template to ensure consistency.

---

## Template

```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added - [Category]

- **[Feature Name]** ([Game Version])
  - Brief description of feature
  - Key details or changes
  - Updated files: `path/to/file1.json`, `path/to/file2.md`
  - See `path/to/file.json#/definitions/Feature` for details

### Changed - [Category]

- **[Change Name]** ([Game Version])
  - Description of what changed
  - Why it changed
  - Updated files: `path/to/file1.json`
  - Migration notes if applicable

### Fixed - [Category]

- **[Fix Name]** ([Game Version])
  - Description of bug fix
  - What was broken
  - How it was fixed
  - Updated files: `path/to/file1.json`

### Updated - [Category]

- **[Update Name]** ([Game Version])
  - Description of update
  - What was updated
  - Updated files: `path/to/file1.json`

### Technical Details

- **Schema Changes**
  - `schemas/file1.json`: Description of change
  - `schemas/file2.json`: Description of change

- **Protocol Updates**
  - `protocols/protocol1.md`: Description of update
  - `protocols/protocol2.md`: Description of update

- **Lifecycle Updates**
  - `lifecycles/lifecycle1.md`: Description of update

---

## Version History

- **[X.Y.Z]** (YYYY-MM-DD): Brief summary of changes
- **[Previous Version]** (YYYY-MM-DD): Previous summary
```

---

## Categories

Use these categories consistently:

- **Permissions** - Permission system changes
- **Combat & Raids** - Combat and raid mechanics
- **Reactor Staking & Validation Delegation** - Reactor staking features
- **Struct Lifecycle** - Struct creation, destruction, lifecycle
- **Fleet Movement** - Fleet movement and navigation
- **Membership Join Process** - Guild membership changes
- **Bug Resolutions** - Bug fixes
- **Documentation** - Documentation-only updates
- **Database Schema** - Database schema changes

---

## Format Guidelines

### Feature Descriptions

- Start with feature name in bold
- Include game version in parentheses
- Use bullet points for details
- Reference updated files
- Include schema references when applicable

### File References

- Use backticks for file paths: `` `schemas/entities.json` ``
- Use JSON schema references: `` `schemas/entities.json#/definitions/Player` ``
- Use relative paths from repository root

### Version Format

- Use semantic versioning: `X.Y.Z`
- Date format: `YYYY-MM-DD`
- Game version format: `vX.Y.Z-beta` or `vX.Y.Z`

---

## Example Entry

```markdown
## [1.2.0] - 2026-02-01

### Added - Permissions

- **New Permission Bit: Admin** (v0.9.0-beta)
  - Added new Admin permission bit to permission system
  - Permission values are bit-based flags that can be combined
  - Admin permission bit value: 128
  - Updated `schemas/game-state.json` with permission bit documentation
  - See `schemas/game-state.json#/definitions/Permission` for details

### Changed - Fleet Movement

- **Fleet Movement Speed** (v0.9.0-beta)
  - Increased base fleet movement speed by 20%
  - Updated movement calculations in `schemas/gameplay.json`
  - Updated `lifecycles/fleet-lifecycle.md` with new speed values
  - Migration: Existing fleets automatically use new speed

### Technical Details

- **Schema Changes**
  - `schemas/game-state.json`: Added Admin permission bit (128)
  - `schemas/gameplay.json`: Updated fleet movement speed constant

- **Protocol Updates**
  - `protocols/gameplay-protocol.md`: Updated fleet movement documentation
```

---

## Checklist

Before finalizing CHANGELOG entry:

- [ ] Version number follows semantic versioning
- [ ] Date is current date
- [ ] All categories are appropriate
- [ ] All file references are correct
- [ ] All schema references are valid
- [ ] Game version is included
- [ ] Technical details section is complete
- [ ] Version history is updated
- [ ] Formatting is consistent with previous entries

---

## Related Documentation

- **CHANGELOG.md** - Current changelog
- **templates/version-update-schema.md** - Schema update template
- **templates/version-update-protocol.md** - Protocol update template
- **MAINTENANCE.md** - Documentation update process

---

*Template Version: 1.0.0*  
*Last Updated: January 1, 2026*

