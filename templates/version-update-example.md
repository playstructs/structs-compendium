# Example Update Template

**Version**: Template  
**Last Updated**: [Current Date]  
**Purpose**: Template for updating example files with new version information

---

## Usage

When updating example files (`examples/*.json`) for a new version, use this template to ensure consistency.

---

## Template Structure

### Example File Header

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "version": "X.Y.Z",
  "title": "Example Title",
  "description": "Example description. Updated for [Game Version].",
  "lastUpdated": "YYYY-MM-DD",
  "gameVersion": "vX.Y.Z-beta",
  "category": "example",
  "examples": [
    {
      "name": "Example Name",
      "description": "Example description",
      "structs:added": "vX.Y.Z-beta",
      "structs:addedDate": "YYYY-MM-DD"
    }
  ]
}
```

---

## Update Checklist

- [ ] Update `version` field to new version
- [ ] Update `lastUpdated` field to current date
- [ ] Update `gameVersion` field if applicable
- [ ] Add new examples for new features
- [ ] Update existing examples if changed
- [ ] Remove deprecated examples
- [ ] Verify JSON validity
- [ ] Verify examples match current API
- [ ] Check entity IDs are valid format
- [ ] Check action names match schemas

---

## Common Update Patterns

### Adding New Examples

```json
{
  "examples": [
    {
      "name": "New Feature Example",
      "description": "Example of new feature added in vX.Y.Z-beta",
      "structs:added": "vX.Y.Z-beta",
      "structs:addedDate": "YYYY-MM-DD",
      "request": {
        "method": "GET",
        "url": "http://localhost:1317/structs/new-endpoint/1-1"
      },
      "response": {
        "status": 200,
        "body": {
          "data": {}
        }
      }
    }
  ]
}
```

### Updating Existing Examples

```json
{
  "examples": [
    {
      "name": "Updated Example",
      "description": "Example updated for vX.Y.Z-beta",
      "structs:updated": "vX.Y.Z-beta",
      "structs:updatedDate": "YYYY-MM-DD",
      "structs:updateNote": "Updated to reflect new API changes",
      "request": {
        "method": "GET",
        "url": "http://localhost:1317/structs/endpoint/1-1",
        "headers": {
          "New-Header": "value"
        }
      },
      "response": {
        "status": 200,
        "body": {
          "data": {
            "newField": "value"
          }
        }
      }
    }
  ]
}
```

### Deprecating Examples

```json
{
  "examples": [
    {
      "name": "Deprecated Example",
      "description": "This example is deprecated. Use 'New Example' instead.",
      "structs:deprecated": "vX.Y.Z-beta",
      "structs:deprecatedDate": "YYYY-MM-DD",
      "structs:replacement": "New Example",
      "structs:deprecationNote": "Replaced by new API endpoint"
    }
  ]
}
```

---

## Example-Specific Updates

### Workflow Examples (`examples/workflows/*.json`)

**Required Updates**:
- Version number
- Last updated date
- New workflow steps
- Updated workflow steps
- Error handling examples

**Example**:
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "version": "1.2.0",
  "title": "New Feature Workflow",
  "description": "Workflow for new feature. Updated for v0.9.0-beta.",
  "lastUpdated": "2026-02-01",
  "gameVersion": "v0.9.0-beta",
  "category": "workflow",
  "workflow": {
    "name": "New Feature Workflow",
    "steps": [
      {
        "step": 1,
        "name": "Step Name",
        "description": "Step description",
        "action": "new-action",
        "structs:added": "v0.9.0-beta"
      }
    ]
  }
}
```

### Error Examples (`examples/errors/*.json`)

**Required Updates**:
- Version number
- Last updated date
- New error examples
- Updated error examples
- Resolved error examples

**Example**:
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "version": "1.2.0",
  "title": "New Error Example",
  "description": "Example of new error. Updated for v0.9.0-beta.",
  "lastUpdated": "2026-02-01",
  "gameVersion": "v0.9.0-beta",
  "category": "error",
  "errors": [
    {
      "name": "New Error",
      "code": 100,
      "message": "Error message",
      "structs:added": "v0.9.0-beta",
      "structs:addedDate": "2026-02-01",
      "request": {
        "method": "POST",
        "url": "http://localhost:1317/cosmos/tx/v1beta1/txs",
        "body": {}
      },
      "response": {
        "status": 400,
        "body": {
          "error": {
            "code": 100,
            "message": "Error message"
          }
        }
      }
    }
  ]
}
```

### Bot Examples (`examples/*-bot.json`)

**Required Updates**:
- Version number
- Last updated date
- New bot capabilities
- Updated bot logic
- New action handlers

**Example**:
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "version": "1.2.0",
  "title": "Example Bot",
  "description": "Example bot implementation. Updated for v0.9.0-beta.",
  "lastUpdated": "2026-02-01",
  "gameVersion": "v0.9.0-beta",
  "category": "bot",
  "bot": {
    "name": "Example Bot",
    "capabilities": [
      "new-capability"
    ],
    "actions": [
      {
        "name": "new-action",
        "handler": "handleNewAction",
        "structs:added": "v0.9.0-beta"
      }
    ]
  }
}
```

---

## Validation Steps

### Before Committing

1. **JSON Validity**
   ```bash
   python3 -m json.tool examples/your-file.json > /dev/null
   ```

2. **Version Consistency**
   - Check version matches CHANGELOG
   - Check version matches related schemas

3. **Entity ID Format**
   - Verify entity IDs follow `type-index` format
   - Check entity IDs are valid

4. **Action Names**
   - Verify action names match `schemas/actions.json`
   - Check action names are correct

5. **Endpoint Paths**
   - Verify endpoint paths match API documentation
   - Check base URLs are correct

6. **Example Accuracy**
   - Verify examples work with current API
   - Check request/response formats match schemas

---

## Example: Complete Example Update

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "version": "1.2.0",
  "title": "Complete Example",
  "description": "Complete example updated for v0.9.0-beta. Added new feature examples.",
  "lastUpdated": "2026-02-01",
  "gameVersion": "v0.9.0-beta",
  "category": "example",
  "examples": [
    {
      "name": "New Feature Example",
      "description": "Example of new feature added in v0.9.0-beta",
      "structs:added": "v0.9.0-beta",
      "structs:addedDate": "2026-02-01",
      "request": {
        "method": "GET",
        "url": "http://localhost:1317/structs/new-endpoint/1-1"
      },
      "response": {
        "status": 200,
        "body": {
          "data": {
            "id": "1-1",
            "newField": "value"
          }
        }
      }
    },
    {
      "name": "Updated Example",
      "description": "Example updated for v0.9.0-beta",
      "structs:updated": "v0.9.0-beta",
      "structs:updatedDate": "2026-02-01",
      "structs:updateNote": "Updated to include newField",
      "request": {
        "method": "GET",
        "url": "http://localhost:1317/structs/endpoint/1-1"
      },
      "response": {
        "status": 200,
        "body": {
          "data": {
            "id": "1-1",
            "existingField": "value",
            "newField": "value"
          }
        }
      }
    }
  ]
}
```

---

## Related Documentation

- **templates/version-update-changelog.md** - CHANGELOG update template
- **templates/version-update-schema.md** - Schema update template
- **templates/version-update-protocol.md** - Protocol update template
- **MAINTENANCE.md** - Documentation update process

---

*Template Version: 1.0.0*  
*Last Updated: January 1, 2026*

