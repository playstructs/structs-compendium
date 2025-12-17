# Market Data Validation

**Version**: 1.0.0  
**Category**: Economic Validation  
**Status**: Stable

## Overview

This document defines validation patterns for market data in Structs, including price validation, trading history validation, and market condition validation.

## Market Data Validation Principles

1. **Price Validation**: Verify prices are within acceptable ranges
2. **Supply/Demand Validation**: Validate supply and demand data
3. **Volume Validation**: Verify trading volumes are accurate
4. **Trend Validation**: Validate price trends and market cycles
5. **Timestamp Validation**: Ensure market data timestamps are current

## Price Validation

### Market Price Validation

**Price Structure**:
```json
{
  "price": {
    "value": "number",
    "minimum": 0,
    "unit": "string",
    "timestamp": "date-time",
    "source": "string"
  }
}
```

**Validation Rules**:
- Price must be positive number (≥ 0)
- Unit must be specified (e.g., "grams per kW", "kW per gram")
- Timestamp must be current (within last 24 hours for real-time data)
- Source must be valid (marketplace, agreement, etc.)

### Price Range Validation

**Validation Checks**:
```json
{
  "priceRange": {
    "currentPrice": {
      "check": "withinRange",
      "range": ["low24h", "high24h"],
      "description": "Current price should be within 24h range"
    },
    "high24h": {
      "check": "greaterThanOrEqual",
      "value": "low24h",
      "description": "24h high must be ≥ 24h low"
    },
    "low24h": {
      "check": "greaterThanOrEqual",
      "value": 0,
      "description": "24h low must be ≥ 0"
    }
  }
}
```

## Market Data Structure Validation

### MarketData Validation

**Required Fields**:
```json
{
  "marketData": {
    "resource": {
      "type": "string",
      "enum": ["AlphaMatter", "Energy", "GuildToken"],
      "required": true
    },
    "currentPrice": {
      "type": "object",
      "required": true,
      "schema": "schemas/markets.json#/markets/MarketPrice"
    },
    "high24h": {
      "type": "number",
      "minimum": 0,
      "required": true
    },
    "low24h": {
      "type": "number",
      "minimum": 0,
      "required": true
    },
    "volume24h": {
      "type": "number",
      "minimum": 0,
      "required": true
    },
    "supply": {
      "type": "number",
      "minimum": 0,
      "required": true
    },
    "demand": {
      "type": "number",
      "minimum": 0,
      "required": true
    },
    "timestamp": {
      "type": "date-time",
      "required": true
    }
  }
}
```

**Validation Rules**:
- All required fields must be present
- All numeric fields must be ≥ 0
- Timestamp must be valid date-time format
- high24h must be ≥ low24h
- currentPrice must be within [low24h, high24h] range (approximately)

## Trading History Validation

### Trade Record Validation

**Trade Structure**:
```json
{
  "trade": {
    "id": {
      "type": "string",
      "required": true,
      "format": "trade-id"
    },
    "resource": {
      "type": "string",
      "enum": ["AlphaMatter", "Energy", "GuildToken"],
      "required": true
    },
    "quantity": {
      "type": "number",
      "minimum": 0,
      "required": true
    },
    "price": {
      "type": "number",
      "minimum": 0,
      "required": true
    },
    "total": {
      "type": "number",
      "minimum": 0,
      "required": true,
      "formula": "total = quantity × price"
    },
    "timestamp": {
      "type": "date-time",
      "required": true
    }
  }
}
```

**Validation Rules**:
- Trade ID must be unique
- Quantity must be positive
- Price must be positive
- Total must equal quantity × price
- Timestamp must be valid and chronological

### Trading History Period Validation

**Period Validation**:
```json
{
  "period": {
    "type": "string",
    "enum": ["1h", "24h", "7d", "30d", "all"],
    "required": true
  },
  "trades": {
    "type": "array",
    "items": "Trade",
    "validation": {
      "allTradesInPeriod": true,
      "chronological": true,
      "noDuplicates": true
    }
  }
}
```

## Guild Token Market Data Validation

### GuildTokenMarketData Validation

**Required Fields**:
```json
{
  "guildTokenMarketData": {
    "guildId": {
      "type": "string",
      "format": "guild-id",
      "required": true
    },
    "currentPrice": {
      "type": "number",
      "minimum": 0,
      "required": true
    },
    "marketCap": {
      "type": "number",
      "minimum": 0,
      "required": true,
      "formula": "marketCap = tokensInCirculation × currentPrice"
    },
    "tokensInCirculation": {
      "type": "number",
      "minimum": 0,
      "required": true
    },
    "collateral": {
      "type": "number",
      "minimum": 0,
      "required": true
    },
    "collateralRatio": {
      "type": "number",
      "minimum": 0,
      "required": true,
      "formula": "collateralRatio = (collateral / tokensInCirculation) × 100%"
    },
    "trustRating": {
      "type": "string",
      "enum": ["high", "medium", "low", "unknown"],
      "required": true
    }
  }
}
```

**Validation Rules**:
- Market cap must equal tokensInCirculation × currentPrice
- Collateral ratio must equal (collateral / tokensInCirculation) × 100%
- All numeric fields must be ≥ 0
- Trust rating must be valid enum value

## Energy Market Data Validation

### EnergyMarketData Validation

**Required Fields**:
```json
{
  "energyMarketData": {
    "currentPrice": {
      "type": "number",
      "minimum": 0,
      "required": true,
      "unit": "Alpha Matter per kW"
    },
    "supply": {
      "type": "number",
      "minimum": 0,
      "required": true,
      "unit": "kW"
    },
    "demand": {
      "type": "number",
      "minimum": 0,
      "required": true,
      "unit": "kW"
    },
    "ephemeral": {
      "type": "boolean",
      "value": true,
      "required": true,
      "description": "Energy is ephemeral - must be consumed immediately"
    }
  }
}
```

**Validation Rules**:
- All numeric fields must be ≥ 0
- Ephemeral must be true (energy cannot be stored)
- Supply and demand must be in kW units
- Price must be in Alpha Matter per kW

## Market Order Validation

### MarketOrder Validation

**Pre-Order Validation**:
```json
{
  "preOrderChecks": [
    {
      "check": "playerOnline",
      "required": true
    },
    {
      "check": "sufficientResources",
      "required": true,
      "description": "Must have resources to sell (or funds to buy)"
    },
    {
      "check": "validPrice",
      "required": true,
      "description": "Price must be within acceptable range"
    },
    {
      "check": "validQuantity",
      "required": true,
      "description": "Quantity must be positive"
    }
  ]
}
```

**Post-Order Validation**:
```json
{
  "postOrderValidation": [
    {
      "validation": "orderId",
      "type": "string",
      "required": true
    },
    {
      "validation": "status",
      "type": "string",
      "enum": ["open", "filled", "cancelled", "partial"],
      "required": true
    },
    {
      "validation": "created",
      "type": "date-time",
      "required": true
    }
  ]
}
```

## Market Condition Validation

### Supply/Demand Validation

**Validation Rules**:
- Supply must be ≥ 0
- Demand must be ≥ 0
- Supply/demand ratio indicates market conditions:
  - Supply > Demand: Oversupply (prices may fall)
  - Supply < Demand: Shortage (prices may rise)
  - Supply = Demand: Balanced market

### Volatility Validation

**Volatility Calculation**:
```json
{
  "volatility": {
    "calculation": "Price standard deviation over period",
    "validation": {
      "type": "number",
      "minimum": 0,
      "description": "Higher volatility = more price fluctuation"
    }
  }
}
```

### Trend Validation

**Trend Types**:
- **Rising**: Prices increasing over time
- **Falling**: Prices decreasing over time
- **Stable**: Prices relatively constant

**Validation**:
- Trend must match price movement pattern
- Trend should be consistent with supply/demand balance

## Common Validation Errors

### Invalid Price

**Error**: `INVALID_PRICE`
- **Check**: Price is outside acceptable range
- **Solution**: Check current market prices, adjust price
- **Validation**: Price must be ≥ 0 and within reasonable range

### Invalid Quantity

**Error**: `INVALID_QUANTITY`
- **Check**: Quantity is not positive
- **Solution**: Ensure quantity > 0
- **Validation**: Quantity must be positive number

### Stale Data

**Error**: `STALE_DATA`
- **Check**: Market data timestamp is too old
- **Solution**: Query fresh market data
- **Validation**: Timestamp should be within last 24 hours for real-time data

### Invalid Market Cap

**Error**: `INVALID_MARKET_CAP`
- **Check**: Market cap doesn't match calculation
- **Solution**: Verify marketCap = tokensInCirculation × currentPrice
- **Validation**: Market cap must equal calculated value

## Best Practices

1. **Always validate price ranges**: Check prices are within 24h high/low
2. **Verify calculations**: Validate market cap, collateral ratio, etc.
3. **Check data freshness**: Ensure market data timestamps are current
4. **Validate supply/demand**: Verify supply and demand are non-negative
5. **Verify formulas**: Check calculated fields match formulas

---

*Last Updated: January 2025*

