# Formula Verification

**Version**: 1.0.0  
**Category**: Economic Validation  
**Status**: Stable

## Overview

This document defines verification patterns for economic formulas in Structs. All formulas should be verified against actual code implementation.

## Verification Principles

1. **Code is the Ultimate Source of Truth**: Always verify formulas against code
2. **Document Discrepancies**: If documentation differs from code, document the discrepancy
3. **Version Tracking**: Track formula versions and changes
4. **Code References**: Include code references for all formulas

## Energy Production Formulas

### Reactor Energy Production

**Formula**:
```
Energy (kW) = Alpha Matter (grams) × 1
```

**Verification Status**: ✅ **VERIFIED**
- **Code Reference**: `genesis_struct_type.go` - Reactor `GeneratingRate` field
- **Rate**: 1 (fixed)
- **Last Verified**: January 2025
- **Status**: Matches code implementation

**Validation**:
```json
{
  "formula": "Energy (kW) = Alpha Matter (grams) × 1",
  "rate": 1,
  "verified": true,
  "codeReference": "genesis_struct_type.go",
  "lastVerified": "2025-01-XX"
}
```

### Generator Energy Production

**Field Generator Formula**:
```
Energy (kW) = Alpha Matter (grams) × 2
```

**Verification Status**: ✅ **VERIFIED**
- **Code Reference**: `genesis_struct_type.go` - Field Generator `GeneratingRate` field
- **Rate**: 2
- **Last Verified**: January 2025
- **Status**: Matches code implementation

**Continental Power Plant Formula**:
```
Energy (kW) = Alpha Matter (grams) × 5
```

**Verification Status**: ✅ **VERIFIED**
- **Code Reference**: `genesis_struct_type.go` - Continental Power Plant `GeneratingRate` field
- **Rate**: 5
- **Last Verified**: January 2025
- **Status**: Matches code implementation

**World Engine Formula**:
```
Energy (kW) = Alpha Matter (grams) × 10
```

**Verification Status**: ✅ **VERIFIED**
- **Code Reference**: `genesis_struct_type.go` - World Engine `GeneratingRate` field
- **Rate**: 10
- **Last Verified**: January 2025
- **Status**: Matches code implementation

**Note**: Previous documentation said "× 2" for all generators, but code verification shows 3 generator types with rates 2, 5, and 10.

## Resource Conversion Formulas

### Alpha Ore to Alpha Matter Conversion

**Formula**:
```
Alpha Matter (grams) = Alpha Ore (grams) × 1
```

**Verification Status**: ⚠️ **NEEDS VERIFICATION**
- **Code Reference**: TBD
- **Rate**: 1 (assumed 1:1 conversion)
- **Last Verified**: TBD
- **Status**: Needs code verification

**Validation**:
```json
{
  "formula": "Alpha Matter (grams) = Alpha Ore (grams) × 1",
  "rate": 1,
  "verified": false,
  "codeReference": "TBD",
  "needsVerification": true
}
```

## Cost Calculation Formulas

### Energy Production Cost

**Reactor Cost**:
```
Cost per kW = Alpha Matter Cost / 1
```

**Generator Costs**:
```
Field Generator: Cost per kW = Alpha Matter Cost / 2
Continental Power Plant: Cost per kW = Alpha Matter Cost / 5
World Engine: Cost per kW = Alpha Matter Cost / 10
```

**Verification Status**: ⚠️ **NEEDS VERIFICATION**
- **Code Reference**: TBD
- **Last Verified**: TBD
- **Status**: Theoretical calculation, needs code verification

## Market Calculation Formulas

### Price Determination

**Formula**:
```
Market Price = f(Supply, Demand, Guild Activities, Combat Outcomes, Resource Discoveries)
```

**Verification Status**: ⚠️ **NEEDS VERIFICATION**
- **Code Reference**: TBD
- **Last Verified**: TBD
- **Status**: Player-driven, exact formula depends on market implementation

### Trading Profit

**Formula**:
```
Profit = (Sell Price - Buy Price) × Quantity
```

**Verification Status**: ✅ **VERIFIED** (Mathematical)
- **Code Reference**: N/A (mathematical calculation)
- **Last Verified**: January 2025
- **Status**: Standard profit calculation

### Profit Margin

**Formula**:
```
Profit Margin = ((Sell Price - Buy Price) / Buy Price) × 100%
```

**Verification Status**: ✅ **VERIFIED** (Mathematical)
- **Code Reference**: N/A (mathematical calculation)
- **Last Verified**: January 2025
- **Status**: Standard margin calculation

## Guild Token Formulas

### Collateral Ratio

**Formula**:
```
Collateral Ratio = (Locked Alpha Matter / Total Tokens Issued) × 100%
```

**Verification Status**: ⚠️ **NEEDS VERIFICATION**
- **Code Reference**: TBD
- **Last Verified**: TBD
- **Status**: Theoretical calculation, needs code verification

### Token Value

**Formula**:
```
Token Value = Locked Alpha Matter / Tokens in Circulation
```

**Verification Status**: ⚠️ **NEEDS VERIFICATION**
- **Code Reference**: TBD
- **Last Verified**: TBD
- **Status**: Theoretical calculation, needs code verification

## Verification Process

### Step 1: Identify Formula

1. Locate formula in documentation
2. Identify formula type (energy production, conversion, cost, market, etc.)
3. Note current verification status

### Step 2: Find Code Reference

1. Search codebase for formula implementation
2. Locate relevant code files
3. Identify code references (file, function, line number)

### Step 3: Verify Formula

1. Compare documentation formula with code implementation
2. Check if rates match
3. Verify calculation logic
4. Document any discrepancies

### Step 4: Update Documentation

1. Update verification status
2. Add code references
3. Document discrepancies (if any)
4. Update last verified date

## Discrepancy Tracking

### Known Discrepancies

**Generator Rates** (RESOLVED):
- **Previous Documentation**: "× 2" for all generators
- **Code Implementation**: 3 generator types with rates 2, 5, and 10
- **Resolution**: Updated documentation to reflect code implementation
- **Date Resolved**: January 2025

## Verification Checklist

For each formula, verify:
- [ ] Formula matches code implementation
- [ ] Rates are correct
- [ ] Code reference is documented
- [ ] Last verified date is current
- [ ] Discrepancies are documented (if any)
- [ ] Formula is testable

## Best Practices

1. **Always verify against code**: Code is the ultimate source of truth
2. **Document code references**: Include file, function, and line number
3. **Track verification dates**: Know when formula was last verified
4. **Document discrepancies**: If docs differ from code, document it
5. **Coordinate with Game Code Analyst**: Work together on verification

---

*Last Updated: January 2025*

