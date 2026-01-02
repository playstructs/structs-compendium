# Schema Validation Procedures

**Version**: 1.0.0  
**Last Updated**: January 1, 2026  
**Purpose**: Procedures for validating schema examples against real API responses

---

## Overview

This document provides comprehensive procedures for validating that schema examples match real API responses. Validation ensures documentation accuracy and helps identify schema discrepancies early.

---

## Validation Scope

### Schemas to Validate

**Entity Schemas**:
- `schemas/entities/*.json` - All entity definitions
- `schemas/entities.json` - Entity catalog

**Action Schemas**:
- `schemas/actions.json` - Action definitions

**Response Schemas**:
- `schemas/responses.json` - Response formats
- `schemas/requests.json` - Request formats

**Error Schemas**:
- `schemas/errors.json` - Error definitions

**Example Schemas**:
- `examples/*.json` - All example files

---

## Validation Checklist

### Pre-Validation Setup

- [ ] Identify all schemas to validate
- [ ] Set up test environment
- [ ] Prepare test data
- [ ] Set up validation tools
- [ ] Prepare validation scripts

---

## Validation Procedures

### 1. Schema Discovery

#### 1.1 List All Schemas

**Sources**:
- `schemas/*.json` - All schema files
- `examples/*.json` - All example files
- `reference/entity-index.json` - Entity index

**Procedure**:
```bash
# List all schema files
find schemas -name "*.json" -type f

# List all example files
find examples -name "*.json" -type f
```

**Output**: List of all schemas and examples to validate

---

### 2. Field Validation

#### 2.1 Verify All Fields Match

**Check**:
- All documented fields exist in actual responses
- All actual response fields are documented
- Field names match exactly
- Field order (if significant)

**Procedure**:
1. Make API request
2. Capture actual response
3. Extract field names from response
4. Compare with schema field names
5. Document missing or extra fields

**Example**:
```json
{
  "validation": {
    "schema": "schemas/entities/player.json",
    "endpoint": "GET /structs/player/1-11",
    "fieldComparison": {
      "documentedFields": [
        "id",
        "index",
        "guildId",
        "substationId",
        "creator",
        "primaryAddress",
        "planetId",
        "fleetId"
      ],
      "actualFields": [
        "id",
        "index",
        "guildId",
        "substationId",
        "creator",
        "primaryAddress",
        "planetId",
        "fleetId",
        "halted"
      ],
      "missingInDocs": ["halted"],
      "missingInResponse": [],
      "match": false
    }
  }
}
```

---

### 3. Data Type Validation

#### 3.1 Verify Data Types Match

**Check**:
- Field types match schemas (string, number, boolean, object, array)
- Nested object types match
- Array element types match
- Nullable fields are correctly marked

**Procedure**:
1. Extract field values from actual response
2. Determine actual data types
3. Compare with schema type definitions
4. Document type mismatches

**Example**:
```json
{
  "validation": {
    "schema": "schemas/entities/player.json",
    "field": "id",
    "documentedType": "string",
    "actualType": "string",
    "match": true
  },
  {
    "field": "index",
    "documentedType": "string",
    "actualType": "number",
    "match": false,
    "discrepancy": "Schema says string, actual is number"
  }
}
```

---

### 4. Enum Validation

#### 4.1 Verify Enum Values Match

**Check**:
- All enum values in responses match documented enums
- All documented enum values appear in responses (if testable)
- Enum values are case-sensitive and match exactly

**Procedure**:
1. Identify enum fields in schemas
2. Collect enum values from actual responses
3. Compare with documented enum values
4. Document mismatches

**Example**:
```json
{
  "validation": {
    "schema": "schemas/gameplay.json",
    "field": "raidStatus",
    "documentedEnums": [
      "pending",
      "in_progress",
      "completed",
      "attackerRetreated"
    ],
    "actualEnums": [
      "pending",
      "in_progress",
      "completed",
      "attackerRetreated"
    ],
    "match": true
  }
}
```

---

### 5. Example Validation

#### 5.1 Validate Example Files

**Check**:
- Example JSON is valid
- Example structure matches schemas
- Example values are realistic
- Example entity IDs are valid format

**Procedure**:
1. Validate JSON syntax
2. Compare example structure with schema
3. Verify entity ID formats
4. Check example values are realistic
5. Document issues

**Example**:
```json
{
  "validation": {
    "example": "examples/workflows/reactor-staking-infuse.json",
    "jsonValid": true,
    "structureMatchesSchema": true,
    "entityIdsValid": true,
    "valuesRealistic": true,
    "issues": []
  }
}
```

---

### 6. Cross-Schema Validation

#### 6.1 Verify Schema References

**Check**:
- All `$ref` references are valid
- Referenced schemas exist
- Schema references are correct
- Cross-references are consistent

**Procedure**:
1. Extract all `$ref` references from schemas
2. Verify referenced files exist
3. Verify referenced paths are valid
4. Check for circular references
5. Document issues

**Example**:
```json
{
  "validation": {
    "schema": "schemas/entities/player.json",
    "references": [
      {
        "ref": "schemas/entities.json#/definitions/Player",
        "exists": true,
        "valid": true
      },
      {
        "ref": "schemas/formats.json#/definitions/EntityID",
        "exists": true,
        "valid": true
      }
    ],
    "allValid": true
  }
}
```

---

## Validation Tools

### JSON Schema Validator

**Tool**: JSON Schema validator (ajv, jsonschema, etc.)

**Usage**:
```bash
# Validate JSON against schema
ajv validate -s schemas/entities/player.json -d examples/player-example.json
```

---

### Custom Validation Script

**Script Template**:
```python
#!/usr/bin/env python3
"""
Schema Validation Script
Validates schemas and examples against actual API responses
"""

import json
import requests
import jsonschema

def validate_schema(schema_path, example_path):
    """Validate example against schema"""
    with open(schema_path) as f:
        schema = json.load(f)
    
    with open(example_path) as f:
        example = json.load(f)
    
    try:
        jsonschema.validate(instance=example, schema=schema)
        return {'valid': True, 'errors': []}
    except jsonschema.ValidationError as e:
        return {'valid': False, 'errors': [str(e)]}

def validate_against_api(schema_path, endpoint):
    """Validate schema against actual API response"""
    response = requests.get(endpoint)
    actual_data = response.json()
    
    with open(schema_path) as f:
        schema = json.load(f)
    
    try:
        jsonschema.validate(instance=actual_data, schema=schema)
        return {'valid': True, 'errors': []}
    except jsonschema.ValidationError as e:
        return {'valid': False, 'errors': [str(e)]}

# Validate all schemas
schemas = [
    ('schemas/entities/player.json', 'examples/player-example.json'),
    # Add more schema/example pairs
]

for schema_path, example_path in schemas:
    result = validate_schema(schema_path, example_path)
    print(f"{schema_path}: {result}")
```

---

## Validation Report Template

### Report Structure

```json
{
  "validationReport": {
    "version": "1.0.0",
    "date": "2026-01-01",
    "validator": "Documentation Team",
    "summary": {
      "totalSchemas": 20,
      "validated": 18,
      "failed": 2,
      "discrepancies": 5
    },
    "schemas": [
      {
        "path": "schemas/entities/player.json",
        "status": "validated",
        "fieldsMatch": true,
        "typesMatch": true,
        "enumsMatch": true,
        "examplesValid": true,
        "discrepancies": []
      }
    ],
    "discrepancies": [
      {
        "schema": "schemas/entities/player.json",
        "type": "missing_field",
        "field": "halted",
        "description": "Response includes 'halted' field not in schema",
        "severity": "low",
        "action": "Add field to schema"
      }
    ]
  }
}
```

---

## Common Issues and Solutions

### Issue: Missing Field in Schema

**Description**: Actual response includes field not documented in schema

**Solution**:
1. Add field to schema
2. Document field purpose
3. Update examples if needed

---

### Issue: Extra Field in Schema

**Description**: Schema includes field not in actual response

**Solution**:
1. Verify field is actually missing
2. Check if field is optional/conditional
3. Remove field if truly not present
4. Update documentation

---

### Issue: Type Mismatch

**Description**: Schema type doesn't match actual type

**Solution**:
1. Verify actual type from API
2. Update schema type
3. Update examples
4. Document change

---

### Issue: Enum Value Mismatch

**Description**: Actual enum value not in documented enum list

**Solution**:
1. Verify actual enum value
2. Add to enum list in schema
3. Update documentation
4. Update examples

---

## Related Documentation

- **verification/api-verification-procedures.md** - API verification procedures
- **verification/discrepancy-checking-procedures.md** - Discrepancy checking procedures
- **schemas/** - Schema files
- **examples/** - Example files

---

*Last Updated: January 1, 2026*

