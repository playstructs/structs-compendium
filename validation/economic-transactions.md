# Economic Transaction Validation

**Version**: 1.0.0  
**Category**: Economic Validation  
**Status**: Stable

## Overview

This document defines validation patterns for economic transactions in Structs, including resource extraction, refinement, energy production, and trading operations.

## Validation Principles

### Resource Security Validation

**Alpha Ore (Stealable)**:
- ⚠️ **Must validate**: Ore is stealable and should be refined immediately
- **Validation Rule**: If ore exists, check if refinement is available
- **Action**: Refine ore to Alpha Matter immediately

**Alpha Matter (Non-Stealable)**:
- ✅ **Validation**: Alpha Matter is secure and cannot be stolen
- **Validation Rule**: Alpha Matter can be safely stored
- **Action**: Safe to use for operations or trading

### Energy Ephemeral Validation

**Energy Properties**:
- ⚠️ **Must validate**: Energy is ephemeral and must be consumed immediately
- **Validation Rule**: Energy cannot be stored
- **Action**: Consume energy immediately upon production

## Transaction Validation Patterns

### Mining Transaction Validation

**Pre-Transaction Checks**:
```json
{
  "checks": [
    {
      "check": "playerOnline",
      "required": true,
      "description": "Player must be online"
    },
    {
      "check": "oreExtractorBuilt",
      "required": true,
      "description": "Ore Extractor struct must be built"
    },
    {
      "check": "planetHasOre",
      "required": true,
      "description": "Planet must have ore available"
    },
    {
      "check": "sufficientCharge",
      "required": true,
      "description": "Struct must have sufficient charge"
    },
    {
      "check": "proofOfWork",
      "required": true,
      "description": "Valid proof-of-work required"
    }
  ]
}
```

**Post-Transaction Validation**:
```json
{
  "validations": [
    {
      "validation": "oreExtracted",
      "type": "number",
      "minimum": 0,
      "description": "Ore amount extracted"
    },
    {
      "validation": "oreStealable",
      "type": "boolean",
      "value": true,
      "description": "Ore is stealable - refine immediately"
    }
  ]
}
```

### Refinement Transaction Validation

**Pre-Transaction Checks**:
```json
{
  "checks": [
    {
      "check": "playerOnline",
      "required": true
    },
    {
      "check": "oreRefineryBuilt",
      "required": true,
      "description": "Ore Refinery struct must be built"
    },
    {
      "check": "hasAlphaOre",
      "required": true,
      "description": "Must have Alpha Ore to refine"
    },
    {
      "check": "sufficientCharge",
      "required": true
    },
    {
      "check": "proofOfWork",
      "required": true
    }
  ]
}
```

**Post-Transaction Validation**:
```json
{
  "validations": [
    {
      "validation": "alphaMatterCreated",
      "type": "number",
      "minimum": 0,
      "formula": "alphaMatterCreated = alphaOreInput × 1",
      "description": "Alpha Matter created (1:1 conversion)"
    },
    {
      "validation": "oreStealable",
      "type": "boolean",
      "value": false,
      "description": "Alpha Matter cannot be stolen"
    }
  ]
}
```

### Energy Production Transaction Validation

**Reactor Validation**:
```json
{
  "preChecks": [
    {
      "check": "playerOnline",
      "required": true
    },
    {
      "check": "reactorBuilt",
      "required": true
    },
    {
      "check": "hasAlphaMatter",
      "required": true,
      "minimum": 1,
      "unit": "grams"
    }
  ],
  "postValidation": [
    {
      "validation": "energyProduced",
      "type": "number",
      "formula": "energyProduced = alphaMatterInput × 1",
      "minimum": 0,
      "unit": "kW"
    },
    {
      "validation": "energyEphemeral",
      "type": "boolean",
      "value": true,
      "description": "Energy must be consumed immediately"
    }
  ]
}
```

**Generator Validation**:
```json
{
  "preChecks": [
    {
      "check": "playerOnline",
      "required": true
    },
    {
      "check": "generatorBuilt",
      "required": true,
      "types": ["fieldGenerator", "continentalPowerPlant", "worldEngine"]
    },
    {
      "check": "hasAlphaMatter",
      "required": true
    }
  ],
  "postValidation": [
    {
      "validation": "energyProduced",
      "type": "number",
      "formula": "energyProduced = alphaMatterInput × generatorRate",
      "generatorRates": {
        "fieldGenerator": 2,
        "continentalPowerPlant": 5,
        "worldEngine": 10
      },
      "minimum": 0,
      "unit": "kW"
    },
    {
      "validation": "energyEphemeral",
      "type": "boolean",
      "value": true
    },
    {
      "validation": "risk",
      "type": "string",
      "value": "high",
      "description": "Generators have higher risk"
    }
  ]
}
```

### Trading Transaction Validation

**Alpha Matter Trading Validation**:
```json
{
  "preChecks": [
    {
      "check": "playerOnline",
      "required": true
    },
    {
      "check": "hasAlphaMatter",
      "required": true,
      "minimum": "tradeQuantity",
      "description": "Must have sufficient Alpha Matter"
    },
    {
      "check": "validMarket",
      "required": true,
      "description": "Market must be available"
    },
    {
      "check": "validPrice",
      "required": true,
      "description": "Price must be within acceptable range"
    }
  ],
  "postValidation": [
    {
      "validation": "tradeExecuted",
      "type": "boolean",
      "value": true
    },
    {
      "validation": "resourcesUpdated",
      "type": "boolean",
      "value": true
    },
    {
      "validation": "alphaMatterStealable",
      "type": "boolean",
      "value": false,
      "description": "Alpha Matter cannot be stolen"
    }
  ]
}
```

**Energy Trading Validation**:
```json
{
  "preChecks": [
    {
      "check": "playerOnline",
      "required": true
    },
    {
      "check": "hasEnergy",
      "required": true,
      "description": "Must have energy to sell (or funds to buy)"
    },
    {
      "check": "energyEphemeral",
      "required": true,
      "warning": "Energy must be consumed immediately - trade quickly"
    }
  ],
  "postValidation": [
    {
      "validation": "tradeExecuted",
      "type": "boolean"
    },
    {
      "validation": "energyReceived",
      "type": "number",
      "unit": "kW",
      "warning": "Must consume immediately"
    }
  ]
}
```

## Formula Validation

### Energy Production Formula Validation

**Reactor Formula**:
```
Energy (kW) = Alpha Matter (grams) × 1
```

**Validation**:
- Input: Alpha Matter (grams) - must be positive number
- Output: Energy (kW) - must equal input × 1
- Rate: 1 (fixed)

**Generator Formulas**:
```
Field Generator: Energy (kW) = Alpha Matter (grams) × 2
Continental Power Plant: Energy (kW) = Alpha Matter (grams) × 5
World Engine: Energy (kW) = Alpha Matter (grams) × 10
```

**Validation**:
- Input: Alpha Matter (grams) - must be positive number
- Output: Energy (kW) - must equal input × generatorRate
- Rate: 2, 5, or 10 (depends on generator type)

### Conversion Formula Validation

**Ore to Matter Conversion**:
```
Alpha Matter (grams) = Alpha Ore (grams) × 1
```

**Validation**:
- Input: Alpha Ore (grams) - must be positive number
- Output: Alpha Matter (grams) - must equal input × 1
- Rate: 1 (fixed, 1:1 conversion)

## Common Validation Errors

### Insufficient Resources

**Error**: `INSUFFICIENT_ALPHA_MATTER`
- **Check**: Verify Alpha Matter balance before transaction
- **Solution**: Mine more ore, refine to Alpha Matter, or trade for Alpha Matter

**Error**: `INSUFFICIENT_ENERGY`
- **Check**: Verify energy availability
- **Solution**: Produce more energy from Alpha Matter
- **Note**: Energy is ephemeral - must be consumed immediately

### Security Validation Errors

**Error**: `ORE_STOLEN`
- **Check**: Verify ore was not stolen before refinement
- **Solution**: Refine ore immediately after mining
- **Prevention**: Build refinery before or immediately after extractor

### Energy Validation Errors

**Error**: `ENERGY_EXPIRED`
- **Check**: Verify energy was consumed before expiration
- **Solution**: Consume energy immediately upon production
- **Prevention**: Have consumption ready before production

## Validation Best Practices

1. **Always validate resource security**:
   - Check if ore is stealable (refine immediately)
   - Verify Alpha Matter is secure (non-stealable)

2. **Always validate energy ephemeral property**:
   - Verify energy must be consumed immediately
   - Check consumption readiness before production

3. **Validate formulas**:
   - Verify conversion rates (1:1 for ore→matter, 1/2/5/10 for matter→energy)
   - Check calculation accuracy

4. **Validate prerequisites**:
   - Check player online status
   - Verify struct requirements
   - Validate resource availability

5. **Post-transaction validation**:
   - Verify transaction success
   - Check resource balance updates
   - Validate security properties

---

*Last Updated: January 2025*

