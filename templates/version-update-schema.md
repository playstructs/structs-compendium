# Schema Update Template

**Version**: Template  
**Last Updated**: [Current Date]  
**Purpose**: Template for updating schema files with new version information

---

## Usage

When updating schema files for a new version, use this template to ensure consistency.

---

## Template Structure

### JSON Schema Header

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "version": "X.Y.Z",
  "title": "Schema Title",
  "description": "Schema description. Updated for [Game Version].",
  "type": "object",
  "structs:category": "category",
  "lastUpdated": "YYYY-MM-DD",
  "gameVersion": "vX.Y.Z-beta"
}
```

### Version Update Checklist

- [ ] Update `version` field to new version
- [ ] Update `lastUpdated` field to current date
- [ ] Update `gameVersion` field if applicable
- [ ] Update description if needed
- [ ] Add new fields/sections
- [ ] Update existing fields if changed
- [ ] Add change notes if needed
- [ ] Verify JSON validity
- [ ] Check cross-references

---

## Common Update Patterns

### Adding New Fields

```json
{
  "properties": {
    "existingField": {
      "type": "string",
      "description": "Existing field"
    },
    "newField": {
      "type": "string",
      "description": "New field added in [Game Version]",
      "structs:added": "vX.Y.Z-beta",
      "structs:addedDate": "YYYY-MM-DD"
    }
  }
}
```

### Updating Existing Fields

```json
{
  "properties": {
    "updatedField": {
      "type": "string",
      "description": "Updated field. Changed in [Game Version].",
      "structs:changed": "vX.Y.Z-beta",
      "structs:changedDate": "YYYY-MM-DD",
      "structs:changeNote": "Brief description of change"
    }
  }
}
```

### Deprecating Fields

```json
{
  "properties": {
    "deprecatedField": {
      "type": "string",
      "description": "Deprecated field. Use 'newField' instead.",
      "structs:deprecated": "vX.Y.Z-beta",
      "structs:deprecatedDate": "YYYY-MM-DD",
      "structs:replacement": "newField"
    }
  }
}
```

---

## Schema-Specific Updates

### Entity Schemas (`schemas/entities/*.json`)

**Required Updates**:
- Version number
- Last updated date
- New entity properties
- Updated entity properties
- Database change metadata (if applicable)

**Example**:
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "version": "1.2.0",
  "title": "Player Entity",
  "description": "Player entity definition. Updated for v0.9.0-beta.",
  "type": "object",
  "structs:category": "entity",
  "lastUpdated": "2026-02-01",
  "gameVersion": "v0.9.0-beta",
  "properties": {
    "id": {
      "type": "string",
      "description": "Player ID (format: type-index)"
    },
    "newProperty": {
      "type": "string",
      "description": "New property added in v0.9.0-beta",
      "structs:added": "v0.9.0-beta",
      "structs:addedDate": "2026-02-01"
    }
  }
}
```

### Action Schemas (`schemas/actions.json`)

**Required Updates**:
- Version number
- Last updated date
- New actions
- Updated actions
- Deprecated actions

**Example**:
```json
{
  "actions": {
    "new-action": {
      "name": "new-action",
      "description": "New action added in v0.9.0-beta",
      "structs:added": "v0.9.0-beta",
      "structs:addedDate": "2026-02-01",
      "parameters": {
        "param1": {
          "type": "string",
          "description": "Parameter description"
        }
      }
    }
  }
}
```

### Error Schemas (`schemas/errors.json`)

**Required Updates**:
- Version number
- Last updated date
- New error codes
- Updated error codes
- Resolved bugs section

**Example**:
```json
{
  "errors": {
    "NEW_ERROR": {
      "code": 100,
      "message": "New error message",
      "description": "Error description",
      "structs:added": "v0.9.0-beta",
      "structs:addedDate": "2026-02-01"
    }
  },
  "resolvedBugs": {
    "v0.9.0-beta": {
      "date": "2026-02-01",
      "bugs": [
        {
          "name": "Bug Name",
          "description": "Bug description",
          "resolved": true
        }
      ]
    }
  }
}
```

---

## Validation Steps

### Before Committing

1. **JSON Validity**
   ```bash
   python3 -m json.tool schemas/your-file.json > /dev/null
   ```

2. **Version Consistency**
   - Check version matches CHANGELOG
   - Check version matches other schemas

3. **Cross-References**
   - Verify all `$ref` references exist
   - Verify all file references exist

4. **Required Fields**
   - `$schema` present
   - `version` present
   - `title` present (if applicable)
   - `description` present (if applicable)

---

## Example: Complete Schema Update

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "version": "1.2.0",
  "title": "Example Schema",
  "description": "Example schema updated for v0.9.0-beta. Added newField property.",
  "type": "object",
  "structs:category": "example",
  "lastUpdated": "2026-02-01",
  "gameVersion": "v0.9.0-beta",
  "properties": {
    "existingField": {
      "type": "string",
      "description": "Existing field"
    },
    "newField": {
      "type": "string",
      "description": "New field added in v0.9.0-beta",
      "structs:added": "v0.9.0-beta",
      "structs:addedDate": "2026-02-01"
    },
    "updatedField": {
      "type": "string",
      "description": "Updated field. Changed in v0.9.0-beta.",
      "structs:changed": "v0.9.0-beta",
      "structs:changedDate": "2026-02-01",
      "structs:changeNote": "Changed from number to string"
    }
  }
}
```

---

## Related Documentation

- **templates/version-update-changelog.md** - CHANGELOG update template
- **templates/version-update-protocol.md** - Protocol update template
- **templates/version-update-example.md** - Example update template
- **templates/schema-consistency-checks.md** - Schema consistency checks
- **MAINTENANCE.md** - Documentation update process

---

*Template Version: 1.0.0*  
*Last Updated: January 1, 2026*

