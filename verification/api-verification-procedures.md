# API Verification Procedures

**Version**: 1.0.0  
**Last Updated**: January 1, 2026  
**Purpose**: Procedures for verifying all documented API endpoints

---

## Overview

This document provides comprehensive procedures for verifying that all documented API endpoints are accurate, functional, and match the documentation. Verification ensures documentation quality and helps identify discrepancies early.

---

## Verification Scope

### Endpoints to Verify

**Consensus Network API** (`http://localhost:1317`):
- All query endpoints (`/structs/*`)
- Transaction submission endpoint (`/cosmos/tx/v1beta1/txs`)
- Account endpoints (`/cosmos/auth/v1beta1/accounts/*`)

**Web Application API** (`http://localhost:8080`):
- All authenticated endpoints (`/api/*`)
- Authentication endpoints (`/api/auth/*`)
- Public endpoints (if any)

**Streaming API** (NATS/GRASS):
- Event subscriptions
- Event schemas
- Connection endpoints

---

## Verification Checklist

### Pre-Verification Setup

- [ ] Identify all documented endpoints
- [ ] Set up test environment (local or staging)
- [ ] Prepare test data (player IDs, entity IDs, etc.)
- [ ] Set up authentication
- [ ] Prepare verification scripts/tools

---

## Verification Procedures

### 1. Endpoint Discovery

#### 1.1 List All Endpoints

**Sources**:
- `api/queries/*.yaml` - Query endpoints
- `api/webapp/*.yaml` - Webapp endpoints
- `api/transactions/*.yaml` - Transaction endpoints
- `reference/api-quick-reference.md` - Quick reference
- `reference/endpoint-index.json` - Endpoint index

**Procedure**:
```bash
# Extract all endpoint paths
grep -r "path:" api/ | grep -oE '/[^"]+'
grep -r "url:" examples/ | grep -oE 'http://[^"]+'
```

**Output**: List of all documented endpoints

---

### 2. Request Format Verification

#### 2.1 Verify Request Parameters

**Check**:
- Parameter names match documentation
- Parameter types match documentation
- Required parameters are documented
- Optional parameters are documented
- Parameter formats match (entity IDs, etc.)

**Procedure**:
1. For each endpoint, extract documented parameters
2. Compare with actual API requirements
3. Test with valid parameters
4. Test with invalid parameters
5. Document any discrepancies

**Example**:
```json
{
  "verification": {
    "endpoint": "GET /structs/player/{id}",
    "documentedParameters": {
      "id": {
        "type": "string",
        "format": "entity-id",
        "pattern": "^1-[0-9]+$",
        "required": true
      }
    },
    "testCases": [
      {
        "name": "Valid player ID",
        "input": "1-11",
        "expectedStatus": 200,
        "actualStatus": 200,
        "match": true
      },
      {
        "name": "Invalid player ID format",
        "input": "invalid",
        "expectedStatus": 400,
        "actualStatus": 400,
        "match": true
      }
    ]
  }
}
```

---

### 3. Response Format Verification

#### 3.1 Verify Response Structure

**Check**:
- Response status codes match documentation
- Response body structure matches schemas
- All documented fields are present
- Field types match schemas
- Enum values match documentation

**Procedure**:
1. Make request to endpoint
2. Capture actual response
3. Compare with documented schema
4. Verify all fields match
5. Document any discrepancies

**Example**:
```json
{
  "verification": {
    "endpoint": "GET /structs/player/1-11",
    "documentedSchema": "schemas/entities/player.json",
    "actualResponse": {
      "Player": {
        "id": "1-11",
        "index": "11",
        "guildId": "0-1"
      }
    },
    "comparison": {
      "fieldsMatch": true,
      "typesMatch": true,
      "enumsMatch": true,
      "discrepancies": []
    }
  }
}
```

---

### 4. Authentication Verification

#### 4.1 Verify Authentication Requirements

**Check**:
- Authentication requirements match documentation
- Authentication methods work correctly
- Session handling works correctly
- Error responses for unauthorized requests

**Procedure**:
1. Test endpoint without authentication
2. Verify expected error response
3. Test endpoint with authentication
4. Verify successful response
5. Test session expiration handling

**Example**:
```json
{
  "verification": {
    "endpoint": "GET /api/player/1-11",
    "authenticationRequired": true,
    "testCases": [
      {
        "name": "Without authentication",
        "headers": {},
        "expectedStatus": 401,
        "actualStatus": 401,
        "match": true
      },
      {
        "name": "With valid session",
        "headers": {
          "Cookie": "PHPSESSID=valid_session_id"
        },
        "expectedStatus": 200,
        "actualStatus": 200,
        "match": true
      },
      {
        "name": "With expired session",
        "headers": {
          "Cookie": "PHPSESSID=expired_session_id"
        },
        "expectedStatus": 401,
        "actualStatus": 401,
        "match": true
      }
    ]
  }
}
```

---

### 5. Error Response Verification

#### 5.1 Verify Error Responses

**Check**:
- Error status codes match documentation
- Error message formats match documentation
- Error codes match `schemas/errors.json`
- Error responses are consistent

**Procedure**:
1. Test with invalid parameters
2. Test with missing required parameters
3. Test with unauthorized access
4. Test with non-existent resources
5. Compare error responses with documentation

**Example**:
```json
{
  "verification": {
    "endpoint": "GET /structs/player/invalid-id",
    "errorTests": [
      {
        "name": "Invalid entity ID format",
        "input": "invalid-id",
        "expectedStatus": 400,
        "expectedError": {
          "code": 8,
          "message": "Invalid location"
        },
        "actualStatus": 400,
        "actualError": {
          "code": 8,
          "message": "Invalid location"
        },
        "match": true
      },
      {
        "name": "Non-existent player",
        "input": "1-99999",
        "expectedStatus": 404,
        "expectedError": {
          "code": 404,
          "message": "Entity not found"
        },
        "actualStatus": 404,
        "actualError": {
          "code": 404,
          "message": "Entity not found"
        },
        "match": true
      }
    ]
  }
}
```

---

### 6. Pagination Verification

#### 6.1 Verify Pagination

**Check**:
- Pagination parameters work correctly
- Pagination response format matches documentation
- Pagination links are correct
- Pagination limits are enforced

**Procedure**:
1. Test endpoint with pagination parameters
2. Verify pagination response structure
3. Test pagination links
4. Test pagination limits
5. Compare with documentation

**Example**:
```json
{
  "verification": {
    "endpoint": "GET /structs/player",
    "paginationTests": [
      {
        "name": "Default pagination",
        "parameters": {},
        "expected": {
          "hasPagination": true,
          "defaultLimit": 100
        },
        "actual": {
          "hasPagination": true,
          "defaultLimit": 100
        },
        "match": true
      },
      {
        "name": "Custom limit",
        "parameters": {
          "limit": 10
        },
        "expected": {
          "limit": 10,
          "results": 10
        },
        "actual": {
          "limit": 10,
          "results": 10
        },
        "match": true
      }
    ]
  }
}
```

---

## Verification Tools

### Manual Verification

**Tools**:
- Browser (for GET requests)
- curl (command line)
- Postman (API testing)
- Custom scripts

**Procedure**:
1. Use tool to make request
2. Capture response
3. Compare with documentation
4. Document findings

---

### Automated Verification

**Tools**:
- Custom verification scripts
- API testing frameworks
- Schema validators

**Script Template**:
```python
#!/usr/bin/env python3
"""
API Verification Script
Verifies documented endpoints against actual API
"""

import requests
import json
import sys

def verify_endpoint(endpoint_config):
    """Verify a single endpoint"""
    method = endpoint_config.get('method', 'GET')
    url = endpoint_config.get('url')
    expected_status = endpoint_config.get('expectedStatus', 200)
    expected_schema = endpoint_config.get('expectedSchema')
    
    try:
        response = requests.request(method, url)
        
        # Check status code
        if response.status_code != expected_status:
            return {
                'endpoint': url,
                'status': 'FAIL',
                'issue': f'Status code mismatch: expected {expected_status}, got {response.status_code}'
            }
        
        # Validate response structure (if schema provided)
        if expected_schema:
            # Load schema and validate
            # (Implementation depends on schema validator)
            pass
        
        return {
            'endpoint': url,
            'status': 'PASS',
            'response': response.json()
        }
    except Exception as e:
        return {
            'endpoint': url,
            'status': 'ERROR',
            'error': str(e)
        }

# Load endpoint configurations
with open('verification/endpoint-configs.json') as f:
    endpoints = json.load(f)

# Verify all endpoints
results = []
for endpoint in endpoints:
    result = verify_endpoint(endpoint)
    results.append(result)

# Report results
print(json.dumps(results, indent=2))
```

---

## Verification Report Template

### Report Structure

```json
{
  "verificationReport": {
    "version": "1.0.0",
    "date": "2026-01-01",
    "verifier": "Documentation Team",
    "environment": "local",
    "summary": {
      "totalEndpoints": 50,
      "verified": 45,
      "failed": 3,
      "skipped": 2,
      "discrepancies": 5
    },
    "endpoints": [
      {
        "id": "player-by-id",
        "method": "GET",
        "path": "/structs/player/{id}",
        "status": "verified",
        "requestFormat": "match",
        "responseFormat": "match",
        "authentication": "n/a",
        "errors": "match",
        "discrepancies": []
      }
    ],
    "discrepancies": [
      {
        "endpoint": "GET /structs/player/{id}",
        "type": "response_field",
        "description": "Response includes 'halted' field not documented",
        "severity": "low",
        "action": "Update documentation"
      }
    ]
  }
}
```

---

## Common Issues and Solutions

### Issue: Endpoint Not Found (404)

**Possible Causes**:
- Endpoint path incorrect
- Endpoint removed/deprecated
- Base URL incorrect

**Solution**:
1. Verify endpoint path in documentation
2. Check if endpoint exists in codebase
3. Update documentation if endpoint removed
4. Fix path if incorrect

---

### Issue: Response Format Mismatch

**Possible Causes**:
- Schema outdated
- API changed
- Documentation incorrect

**Solution**:
1. Compare actual response with schema
2. Update schema if API changed
3. Update documentation if schema incorrect
4. Document discrepancy

---

### Issue: Authentication Not Working

**Possible Causes**:
- Authentication method changed
- Session handling changed
- Documentation outdated

**Solution**:
1. Test authentication flow
2. Update authentication documentation
3. Verify session handling
4. Document changes

---

## Related Documentation

- **verification/schema-validation-procedures.md** - Schema validation procedures
- **verification/discrepancy-checking-procedures.md** - Discrepancy checking procedures
- **protocols/testing-protocol.md** - Testing protocol
- **api/queries/** - Query endpoint documentation
- **api/webapp/** - Webapp endpoint documentation

---

*Last Updated: January 1, 2026*

