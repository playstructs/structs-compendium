# Protocol Update Template

**Version**: Template  
**Last Updated**: [Current Date]  
**Purpose**: Template for updating protocol documentation files with new version information

---

## Usage

When updating protocol files (`protocols/*.md`) for a new version, use this template to ensure consistency.

---

## Template Header

```markdown
# Protocol Name

**Version**: X.Y.Z  
**Category**: [Category]  
**Status**: [Stable|Beta|Deprecated]  
**Last Updated**: [Current Date]  
**Game Version**: vX.Y.Z-beta
```

---

## Update Checklist

- [ ] Update version number in header
- [ ] Update "Last Updated" date
- [ ] Update game version if applicable
- [ ] Add new sections for new features
- [ ] Update existing sections if changed
- [ ] Add migration notes if applicable
- [ ] Update examples if needed
- [ ] Update cross-references
- [ ] Verify all links work
- [ ] Check formatting consistency

---

## Common Update Patterns

### Adding New Sections

```markdown
## New Feature Name

**Added**: vX.Y.Z-beta  
**Status**: Stable

Description of new feature.

### Usage

How to use the new feature.

### Example

```json
{
  "example": "data"
}
```

### Related Documentation

- `schemas/feature.json` - Feature schema
- `examples/feature-example.json` - Feature example
```

### Updating Existing Sections

```markdown
## Existing Feature

**Updated**: vX.Y.Z-beta

Description of feature. [What changed in this version.]

### Changes

- Change 1: Description
- Change 2: Description

### Migration Notes

If migrating from previous version:
- Step 1: Description
- Step 2: Description
```

### Deprecating Sections

```markdown
## Deprecated Feature

**Deprecated**: vX.Y.Z-beta  
**Replacement**: [New Feature Name]

This feature is deprecated. Use [New Feature Name] instead.

### Migration

How to migrate from deprecated feature to new feature.
```

---

## Protocol-Specific Updates

### Action Protocol (`protocols/action-protocol.md`)

**When to Update**:
- New actions added
- Action parameters changed
- Transaction flow changed
- Error handling changed

**Update Pattern**:
```markdown
## New Action

**Added**: vX.Y.Z-beta

Description of new action.

### Request Format

```json
{
  "action": "new-action",
  "parameters": {
    "param1": "value1"
  }
}
```

### Response Format

```json
{
  "status": "success",
  "data": {}
}
```

### Error Handling

Common errors and how to handle them.
```

### Query Protocol (`protocols/query-protocol.md`)

**When to Update**:
- New endpoints added
- Endpoint parameters changed
- Response format changed
- Pagination changed

**Update Pattern**:
```markdown
## New Endpoint

**Added**: vX.Y.Z-beta

`GET /structs/new-endpoint/{id}`

### Parameters

- `id` (required): Entity ID

### Response

```json
{
  "data": {}
}
```
```

### Gameplay Protocol (`protocols/gameplay-protocol.md`)

**When to Update**:
- New gameplay mechanics
- Combat changes
- Resource changes
- Status changes

**Update Pattern**:
```markdown
## New Gameplay Feature

**Added**: vX.Y.Z-beta

Description of new gameplay feature.

### Mechanics

How the feature works.

### Examples

```json
{
  "example": "data"
}
```
```

### Economic Protocol (`protocols/economic-protocol.md`)

**When to Update**:
- New economic features
- Staking changes
- Trading changes
- Resource economics changes

**Update Pattern**:
```markdown
## New Economic Feature

**Added**: vX.Y.Z-beta

Description of new economic feature.

### Economics

Economic calculations and formulas.

### Examples

```json
{
  "example": "data"
}
```
```

---

## Version History Section

Add version history at the end of protocol file:

```markdown
---

## Version History

- **X.Y.Z** (YYYY-MM-DD): Brief summary of changes
- **Previous Version** (YYYY-MM-DD): Previous summary
```

---

## Example: Complete Protocol Update

```markdown
# Example Protocol

**Version**: 1.2.0  
**Category**: Example  
**Status**: Stable  
**Last Updated**: February 1, 2026  
**Game Version**: v0.9.0-beta

## Overview

Protocol description.

## New Feature

**Added**: v0.9.0-beta

Description of new feature.

### Usage

How to use the feature.

### Example

```json
{
  "example": "data"
}
```

## Updated Feature

**Updated**: v0.9.0-beta

Description of updated feature.

### Changes

- Change 1: Description
- Change 2: Description

---

## Version History

- **1.2.0** (2026-02-01): Added new feature, updated existing feature
- **1.1.0** (2026-01-01): Previous changes
```

---

## Validation Steps

### Before Committing

1. **Format Check**
   - Markdown syntax is valid
   - Code blocks are properly formatted
   - Links are valid

2. **Version Consistency**
   - Version matches CHANGELOG
   - Version matches related schemas
   - Date is current

3. **Cross-References**
   - All file references exist
   - All schema references are valid
   - All example references exist

4. **Content Accuracy**
   - Information matches schemas
   - Examples are correct
   - Migration notes are accurate

---

## Related Documentation

- **templates/version-update-changelog.md** - CHANGELOG update template
- **templates/version-update-schema.md** - Schema update template
- **templates/version-update-example.md** - Example update template
- **MAINTENANCE.md** - Documentation update process

---

*Template Version: 1.0.0*  
*Last Updated: January 1, 2026*

