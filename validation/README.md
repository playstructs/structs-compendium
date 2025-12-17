# Validation

**Version**: 1.0.0  
**Category**: Validation  
**Status**: Stable

## Overview

This directory contains validation patterns and guides for AI agents interacting with Structs. Understanding validation is **critical** for correctly handling action results and avoiding common mistakes.

**⚠️ IMPORTANT**: Transactions can broadcast but fail validation on-chain. Always verify game state after transactions!

---

## Quick Start

1. **Read Transaction Flow**: `transaction-flow.md` - Understand how validation works
2. **Learn Verification**: `verification.md` - How to verify actions succeeded
3. **Check Common Failures**: `common-failures.md` - Common validation failures and solutions

---

## Files in This Directory

### Core Documents

- **`transaction-flow.md`** - Complete transaction validation flow
- **`verification.md`** - How to verify actions actually occurred
- **`common-failures.md`** - Common validation failures and solutions

### Specialized Documents

- **`economic-transactions.md`** - Economic transaction validation (Economics Specialist) ✅
- **`formula-verification.md`** - Formula verification patterns (Economics Specialist) ✅
- **`market-data-validation.md`** - Market data validation (Economics Specialist) ✅

---

## Key Concepts

### Transaction Validation Flow

1. **Create transaction** (pending)
2. **Sign transaction**
3. **Submit to blockchain** (broadcast)
4. **On-chain validation** (checks requirements)
5. **Action succeeds OR fails** (based on validation)
6. **Verify game state** (confirm action occurred)

### Critical Understanding

**A transaction showing "broadcast" status does NOT mean the action succeeded!**

Validation happens on-chain AFTER broadcast. If requirements aren't met, the transaction will broadcast but the action won't happen.

### Verification Pattern

Always verify game state after transaction broadcast:
1. Submit transaction
2. Wait for broadcast status
3. Query game state to verify action occurred
4. If action didn't occur, check requirements
5. Fix requirements and retry if needed

---

## Common Validation Failures

### 1. Planet Building Without Command Ship

**Symptom**: Transaction broadcasts but struct not created  
**Cause**: Command Ship not built or not online  
**Solution**: Build and activate Command Ship first

### 2. Exploration Without Completing Current Planet

**Symptom**: Transaction broadcasts but planet ownership unchanged  
**Cause**: Current planet still has ore remaining  
**Solution**: Mine all ore from current planet first

### 3. Building Without Sufficient Power

**Symptom**: Transaction broadcasts but struct not created  
**Cause**: Insufficient power capacity  
**Solution**: Increase power capacity or reduce load

---

## Best Practices

1. **Always Verify Game State**: Query game state after transactions
2. **Check Requirements Before Acting**: Verify all requirements met before submitting
3. **Handle Validation Failures Gracefully**: Detect failures, identify missing requirements, fix and retry
4. **Use Validation Patterns**: Follow patterns in this directory
5. **Reference Schemas**: Check action requirements in `schemas/actions.json`

---

## Related Documentation

- **Schemas**: `schemas/actions.json` - Action requirements
- **Patterns**: `patterns/validation-patterns.md` - Detailed validation patterns
- **Protocols**: `protocols/action-protocol.md` - How to perform actions
- **Tasks**: `tasks/` - Task workflows with validation steps

---

*Last Updated: January 2025*

