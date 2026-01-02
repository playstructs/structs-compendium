# Schema Consistency Checks

**Version**: 1.0.0  
**Last Updated**: January 1, 2026  
**Purpose**: Guide for automated schema consistency checks

---

## Overview

This document provides a checklist and procedures for verifying schema consistency across the documentation. These checks should be run regularly to ensure documentation quality.

---

## Automated Checks

### 1. Version Number Consistency

**Check**: All version numbers should match the current release version.

**Files to Check**:
- All `schemas/*.json` files
- All `protocols/*.md` files
- All `api/**/*.yaml` files
- `AGENTS.md`
- `CHANGELOG.md`
- `DOCUMENTATION_INDEX.md`

**Procedure**:
```bash
# Find all version numbers
grep -r "\"version\":" schemas/ | grep -v "1.1.0"
grep -r "Version\*\*:" protocols/ | grep -v "1.1.0"
grep -r "version:" api/ | grep -v "1.1.0"
```

**Expected Format**:
- JSON: `"version": "1.1.0"`
- Markdown: `**Version**: 1.1.0`

---

### 2. Last Updated Date Consistency

**Check**: All "Last Updated" dates should be current or recent.

**Files to Check**:
- All `protocols/*.md` files
- All `schemas/*.json` files (if they have lastUpdated field)
- `AGENTS.md`
- `NEXT_STEPS.md`

**Procedure**:
```bash
# Find all last updated dates
grep -r "Last Updated" protocols/ schemas/ *.md
```

**Expected Format**:
- Markdown: `**Last Updated**: January 1, 2026`
- JSON: `"lastUpdated": "2026-01-01"`

---

### 3. Cross-Reference Validation

**Check**: All file references should point to existing files.

**Common Reference Patterns**:
- `schemas/entities.json#/definitions/Player`
- `protocols/action-protocol.md`
- `examples/workflows/*.json`
- `api/queries/player.yaml`

**Procedure**:
```bash
# Extract all file references
grep -rohE '(schemas|protocols|examples|api|lifecycles|patterns|troubleshooting)/[^"#\s]+' . --include="*.md" --include="*.json" --include="*.yaml" | sort -u
```

**Validation**:
- Check each referenced file exists
- Check JSON schema references are valid (`#/definitions/...`)
- Check markdown links are valid

---

### 4. JSON Schema Validation

**Check**: All JSON schema files should be valid JSON Schema.

**Files to Check**:
- All `schemas/*.json` files
- All `examples/*.json` files
- All `reference/*.json` files

**Procedure**:
```bash
# Validate JSON syntax
find schemas examples reference -name "*.json" -exec python3 -m json.tool {} \; > /dev/null

# Validate JSON Schema (if schema validator available)
# npm install -g ajv-cli
# ajv validate -s schemas/entities.json -d schemas/entities/player.json
```

**Expected**:
- Valid JSON syntax
- Valid JSON Schema structure (if applicable)
- Required fields present
- Type definitions correct

---

### 5. Broken Link Detection

**Check**: All markdown links should point to existing files or valid URLs.

**Procedure**:
```bash
# Extract markdown links
grep -rohE '\[([^\]]+)\]\(([^)]+)\)' . --include="*.md" | cut -d: -f2

# Check internal links
# (Requires link checker tool)
```

**Validation**:
- Internal file links should exist
- External URLs should be accessible (optional check)
- Anchor links should match headings

---

### 6. Entity ID Format Consistency

**Check**: All entity IDs should follow the format `type-index`.

**Files to Check**:
- All `schemas/entities/*.json` files
- All `examples/*.json` files
- All `api/queries/*.yaml` files

**Procedure**:
```bash
# Find entity ID patterns
grep -rE '"id"\s*:\s*"[0-9]+-[0-9]+"' schemas/ examples/
```

**Expected Format**:
- `"1-11"` (player type 1, index 11)
- `"2-1"` (planet type 2, index 1)
- See `schemas/formats.json` for complete format specifications

---

### 7. Action Name Consistency

**Check**: All action names should match between schemas and examples.

**Files to Check**:
- `schemas/actions.json`
- `reference/action-index.json`
- `examples/*.json` files with actions

**Procedure**:
```bash
# Extract action names from schema
grep -r '"action"' schemas/actions.json | grep -oE '"[^"]+"' | sort -u

# Extract action names from examples
grep -r '"action"' examples/ | grep -oE '"[^"]+"' | sort -u

# Compare the two lists
```

**Expected**:
- All actions in examples should exist in `schemas/actions.json`
- Action names should match exactly (case-sensitive)

---

### 8. Endpoint Path Consistency

**Check**: All endpoint paths should match between API documentation and examples.

**Files to Check**:
- `api/queries/*.yaml`
- `api/webapp/*.yaml`
- `reference/api-quick-reference.md`
- `examples/*.json` files

**Procedure**:
```bash
# Extract endpoint paths from YAML
grep -r "path:" api/ | grep -oE '/[^"]+'

# Extract endpoint paths from examples
grep -r '"url"' examples/ | grep -oE 'http://[^"]+'

# Compare paths
```

**Expected**:
- All endpoint paths in examples should match API documentation
- Base URLs should be consistent

---

### 9. Error Code Consistency

**Check**: All error codes should be documented in `schemas/errors.json`.

**Files to Check**:
- `schemas/errors.json`
- `api/error-codes.yaml`
- `troubleshooting/error-codes.md`
- `examples/errors/*.json`

**Procedure**:
```bash
# Extract error codes from schema
grep -r '"code"' schemas/errors.json | grep -oE '[0-9]+'

# Extract error codes from examples
grep -r '"code"' examples/errors/ | grep -oE '[0-9]+'

# Compare lists
```

**Expected**:
- All error codes in examples should exist in `schemas/errors.json`
- Error messages should match

---

### 10. Date Format Consistency

**Check**: All dates should use consistent format.

**Expected Formats**:
- ISO 8601: `2026-01-01`
- Human-readable: `January 1, 2026`

**Procedure**:
```bash
# Find all date patterns
grep -rE '[0-9]{4}-[0-9]{2}-[0-9]{2}' .
grep -rE '(January|February|March|April|May|June|July|August|September|October|November|December) [0-9]+, [0-9]{4}' .
```

**Expected**:
- JSON files: ISO 8601 format
- Markdown files: Human-readable format
- No mixed formats

---

## Manual Checks

### 1. Content Accuracy

**Check**: Documentation should accurately reflect the current game state.

**Procedure**:
- Review recent CHANGELOG entries
- Compare documentation to actual API responses
- Verify examples work with current API

---

### 2. Completeness

**Check**: All new features should be documented.

**Procedure**:
- Review NEXT_STEPS.md for pending tasks
- Check CHANGELOG for new features
- Verify all new features have documentation

---

### 3. Cross-Reference Accuracy

**Check**: All cross-references should be accurate and helpful.

**Procedure**:
- Follow each cross-reference
- Verify it points to relevant information
- Check that referenced sections exist

---

## Automation Script

### Basic Check Script

```bash
#!/bin/bash
# schema-consistency-check.sh

echo "Running schema consistency checks..."

# Check version numbers
echo "Checking version numbers..."
VERSION_MISMATCHES=$(grep -r "\"version\":" schemas/ | grep -v "1.1.0" | wc -l)
if [ $VERSION_MISMATCHES -gt 0 ]; then
  echo "⚠️  Found $VERSION_MISMATCHES version mismatches"
else
  echo "✅ All version numbers consistent"
fi

# Check JSON validity
echo "Checking JSON validity..."
find schemas examples reference -name "*.json" -exec python3 -m json.tool {} \; > /dev/null 2>&1
if [ $? -eq 0 ]; then
  echo "✅ All JSON files valid"
else
  echo "⚠️  Some JSON files invalid"
fi

# Check for broken internal links (basic check)
echo "Checking for common broken links..."
BROKEN_LINKS=$(grep -r "schemas/entities.json#/definitions/" . --include="*.md" | grep -v "Player\|Planet\|Struct\|Fleet\|Guild\|Reactor\|Substation\|Provider\|Agreement\|Allocation\|Address\|Infusion" | wc -l)
if [ $BROKEN_LINKS -gt 0 ]; then
  echo "⚠️  Found $BROKEN_LINKS potentially broken entity references"
else
  echo "✅ No obvious broken entity references"
fi

echo "Checks complete!"
```

---

## Running Checks

### Before Commits

Run basic checks before committing:
```bash
./scripts/schema-consistency-check.sh
```

### Weekly Reviews

Run full checks weekly:
- All automated checks
- Sample manual checks
- Review NEXT_STEPS.md progress

### Before Releases

Run comprehensive checks:
- All automated checks
- All manual checks
- Full cross-reference validation
- Full example validation

---

## Fixing Issues

### Version Number Mismatches

1. Identify all files with mismatched versions
2. Update to current version
3. Update "Last Updated" date
4. Verify cross-references still work

### Broken Links

1. Identify broken link
2. Find correct file path
3. Update link
4. Verify link works

### JSON Schema Errors

1. Identify invalid JSON
2. Fix syntax errors
3. Validate structure
4. Test with examples

---

## Related Documentation

- **MAINTENANCE.md** - Documentation update process
- **templates/version-update-*.md** - Version update templates
- **NEXT_STEPS.md** - Pending documentation tasks

---

*Last Updated: January 1, 2026*

