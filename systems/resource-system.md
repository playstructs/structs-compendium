# Resource System

**Version**: 1.0.0  
**Category**: System  
**Status**: Stable

## Overview

The resource system manages raw materials (ore), refined materials (Alpha Matter), and energy (Watts). Understanding resource security and flow is critical for successful gameplay.

---

## Resource Types

### Stored Ore

**Source**: Mined from planets

**Storage**: On planet (stealable)

**Security**: ⚠️ **VULNERABLE** - Can be stolen in raids

**Action**: Refine immediately to secure

### Alpha Matter

**Source**: Refined from ore

**Storage**: In bank account (secure)

**Security**: ✅ **SECURE** - Cannot be stolen

**Action**: Maintain reserve (20-30%)

### Watts

**Source**: Converted from Alpha Matter

**Storage**: Player power system

**Security**: ✅ **SECURE** - Cannot be stolen

**Action**: Use for operations

---

## Resource Flow

### Mining Flow

**From**: Planet ore deposits  
**To**: Stored ore on planet  
**Security**: Stealable  
**Action**: Refine immediately

### Refinement Flow

**From**: Stored ore  
**To**: Alpha Matter (bank account)  
**Security**: Secure  
**Action**: Maintain reserve

### Conversion Flow

**From**: Alpha Matter  
**To**: Watts  
**Methods**: Reactor (1:1) or Generator (1:2)  
**Action**: Convert when needed

---

## Conversion Rates

### Reactor

**Rate**: 1 gram Alpha Matter = 1 kilowatt  
**Risk**: Low (safe, reliable)  
**Use Case**: General conversion

### Planetary Generator

**Rate**: 1 gram Alpha Matter = 2 kilowatts  
**Risk**: High (efficient but risky)  
**Use Case**: When efficiency needed

---

## Resource Security

### Ore Security

**Status**: Stealable  
**Risk**: High  
**Action**: Refine immediately

**Why**: Ore stored on planet can be stolen in raids. Always refine ore as soon as it's mined.

### Alpha Matter Security

**Status**: Secure  
**Risk**: None  
**Action**: Maintain reserve (20-30%)

**Why**: Alpha Matter stored in bank account cannot be stolen. Safe to accumulate.

### Watts Security

**Status**: Secure  
**Risk**: None  
**Action**: Use for operations

**Why**: Watts are part of power system, cannot be stolen.

---

## Resource Management Principles

1. **Always refine ore immediately** - Secure resources
2. **Maintain 20-30% Alpha Matter reserve** - Emergency buffer
3. **Convert Alpha Matter to Watts when needed** - Don't convert all
4. **Monitor resource flow continuously** - Track production and consumption
5. **Never leave ore unrefined** - Security risk

---

## Common Mistakes

1. **Leaving ore unrefined** - Can be stolen
2. **Converting all Alpha Matter** - No reserve
3. **Not monitoring resource flow** - Unaware of issues
4. **Choosing wrong conversion method** - Risk vs efficiency

---

## Related Documentation

- **Tasks**: `../tasks/resource-management.json` - Resource management workflow
- **Troubleshooting**: `../troubleshooting/common-issues.md` - Common issues

---

*Last Updated: January 2025*

