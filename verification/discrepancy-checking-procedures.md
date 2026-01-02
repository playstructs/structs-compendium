# Discrepancy Checking Procedures

**Version**: 1.0.0  
**Last Updated**: January 1, 2026  
**Purpose**: Procedures for checking documentation discrepancies and ensuring all changes are documented

---

## Overview

This document provides comprehensive procedures for checking discrepancies between documentation and code, and ensuring all changes are properly documented. Regular discrepancy checking maintains documentation accuracy.

---

## Discrepancy Types

### Critical Discrepancies

**Impact**: Affects player decisions or game mechanics

**Examples**:
- Incorrect formulas
- Wrong resource rates
- Incorrect status values
- Missing critical information

---

### Medium Discrepancies

**Impact**: Missing information or minor inaccuracies

**Examples**:
- Missing optional fields
- Undocumented constants
- Minor type mismatches

---

### Low Discrepancies

**Impact**: Minor documentation issues

**Examples**:
- Formatting inconsistencies
- Typos
- Outdated examples

---

## Checking Procedures

### 1. Review Existing Discrepancies

#### 1.1 Review `verification/discrepancies.json`

**Procedure**:
1. Load discrepancies file
2. Review all documented discrepancies
3. Check status of each discrepancy
4. Verify fixes for resolved discrepancies
5. Prioritize unresolved discrepancies

**Checklist**:
- [ ] All critical discrepancies reviewed
- [ ] All medium discrepancies reviewed
- [ ] All low discrepancies reviewed
- [ ] Fix status verified
- [ ] Priority assigned

---

### 2. Compare Documentation to Code

#### 2.1 Code Review Process

**Procedure**:
1. Identify code files to review
2. Extract relevant code sections
3. Compare with documentation
4. Document discrepancies
5. Prioritize discrepancies

**Checklist**:
- [ ] Entity definitions compared
- [ ] Action definitions compared
- [ ] Formula implementations compared
- [ ] Constant values compared
- [ ] Status enums compared

---

### 3. Verify All Changes Documented

#### 3.1 Change Documentation Verification

**Procedure**:
1. Review game version changelog
2. Identify all changes
3. Check if changes are documented
4. Document missing documentation
5. Create documentation tasks

**Checklist**:
- [ ] All new features documented
- [ ] All breaking changes documented
- [ ] All bug fixes documented
- [ ] All deprecated features documented
- [ ] All schema changes documented

---

### 4. Check for Missing Information

#### 4.1 Information Completeness Check

**Procedure**:
1. Review all entity schemas
2. Check for missing fields
3. Check for missing examples
4. Check for missing protocols
5. Document missing information

**Checklist**:
- [ ] All entities fully documented
- [ ] All actions fully documented
- [ ] All endpoints fully documented
- [ ] All examples present
- [ ] All protocols complete

---

## Discrepancy Documentation

### Discrepancy Entry Format

```json
{
  "id": "unique-discrepancy-id",
  "category": "economic|resource|gameplay|api|schema",
  "severity": "critical|medium|low",
  "description": "Clear description of discrepancy",
  "documentation": {
    "location": "path/to/file.md or path/to/file.json",
    "current": "What documentation currently says",
    "section": "Section name if applicable"
  },
  "code": {
    "location": "path/to/code.go or path/to/code.js",
    "actual": "What code actually does",
    "line": 123
  },
  "impact": "Description of impact on users/agents",
  "status": "documented|fixed|pending",
  "fixStatus": "pending|in-progress|complete",
  "fixOwner": "Person/team responsible",
  "reportedBy": "Person who found discrepancy",
  "reportedDate": "YYYY-MM-DD",
  "fixedDate": "YYYY-MM-DD (if fixed)",
  "verification": {
    "verified": false,
    "verifiedBy": "Person who verified fix",
    "verifiedDate": "YYYY-MM-DD"
  }
}
```

---

### Adding New Discrepancies

**Procedure**:
1. Identify discrepancy
2. Document discrepancy using format above
3. Add to `verification/discrepancies.json`
4. Assign priority and owner
5. Update summary statistics

**Example**:
```json
{
  "id": "new-discrepancy-id",
  "category": "api",
  "severity": "medium",
  "description": "Endpoint response includes field not in schema",
  "documentation": {
    "location": "schemas/entities/player.json",
    "current": "Schema does not include 'halted' field",
    "section": "Player Properties"
  },
  "code": {
    "location": "api/player.go",
    "actual": "Response includes 'halted' boolean field",
    "line": 45
  },
  "impact": "Agents may not handle 'halted' field correctly",
  "status": "documented",
  "fixStatus": "pending",
  "fixOwner": "Documentation Team",
  "reportedBy": "API Verification",
  "reportedDate": "2026-01-01"
}
```

---

## Discrepancy Resolution

### Resolution Process

**Steps**:
1. Review discrepancy
2. Determine correct value (code or documentation)
3. Fix documentation or code
4. Verify fix
5. Update discrepancy status
6. Close discrepancy

**Checklist**:
- [ ] Discrepancy reviewed
- [ ] Correct value determined
- [ ] Fix implemented
- [ ] Fix verified
- [ ] Status updated
- [ ] Discrepancy closed

---

### Fix Verification

**Procedure**:
1. Review fix implementation
2. Compare fixed documentation with code
3. Verify fix is complete
4. Test fix if applicable
5. Update verification status

**Checklist**:
- [ ] Fix implemented correctly
- [ ] Documentation matches code
- [ ] Examples updated if needed
- [ ] Fix tested
- [ ] Verification complete

---

## Regular Discrepancy Checks

### Weekly Checks

**Scope**: Critical discrepancies

**Tasks**:
- Review critical discrepancies
- Check fix status
- Verify resolved discrepancies
- Update priority if needed

**Duration**: 30 minutes

---

### Monthly Checks

**Scope**: All discrepancies

**Tasks**:
- Review all discrepancies
- Compare documentation to code (sample)
- Check for new discrepancies
- Update discrepancy file

**Duration**: 2-4 hours

---

### Quarterly Checks

**Scope**: Comprehensive review

**Tasks**:
- Full code-to-documentation comparison
- Complete discrepancy audit
- Review discrepancy process
- Update procedures if needed

**Duration**: 1-2 days

---

## Discrepancy Tracking

### Summary Statistics

**Track**:
- Total discrepancies
- By severity (critical, medium, low)
- By category (economic, resource, gameplay, etc.)
- By status (documented, fixed, pending)
- Resolution time

**Update**: After each discrepancy check

---

### Reporting

**Report Format**:
```json
{
  "discrepancyReport": {
    "version": "1.0.0",
    "date": "2026-01-01",
    "summary": {
      "total": 10,
      "critical": 2,
      "medium": 5,
      "low": 3,
      "fixed": 3,
      "pending": 7
    },
    "byCategory": {
      "economic": 3,
      "resource": 2,
      "gameplay": 2,
      "api": 2,
      "schema": 1
    },
    "byStatus": {
      "documented": 7,
      "fixed": 3,
      "pending": 7
    },
    "recentFixes": [
      {
        "id": "discrepancy-id",
        "fixedDate": "2026-01-01",
        "fixOwner": "Documentation Team"
      }
    ]
  }
}
```

---

## Common Discrepancies

### Formula Discrepancies

**Pattern**: Documentation formula doesn't match code implementation

**Example**: Documentation says `rate × 2`, code uses different rates (2, 5, 10)

**Solution**: Update documentation to match code

---

### Field Discrepancies

**Pattern**: Response includes field not in schema, or schema includes field not in response

**Example**: Response includes `halted` field not documented in schema

**Solution**: Add field to schema or remove from response

---

### Type Discrepancies

**Pattern**: Schema type doesn't match actual type

**Example**: Schema says `string`, actual is `number`

**Solution**: Update schema type to match actual

---

### Enum Discrepancies

**Pattern**: Actual enum value not in documented enum list

**Example**: Response includes `attackerRetreated` status not in enum list

**Solution**: Add enum value to schema

---

## Related Documentation

- **verification/discrepancies.json** - Current discrepancies
- **verification/api-verification-procedures.md** - API verification
- **verification/schema-validation-procedures.md** - Schema validation
- **MAINTENANCE.md** - Documentation maintenance process

---

*Last Updated: January 1, 2026*

