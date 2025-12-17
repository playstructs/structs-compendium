# API Usage Guide

**Version**: 1.0.0  
**Last Updated**: December 7, 2025  
**Purpose**: Comprehensive guide for AI agents on using Structs APIs effectively

---

## Overview

This guide provides practical guidance for AI agents on how to effectively use Structs APIs. It covers common scenarios, best practices, and patterns for successful API integration.

---

## Getting Started

### 1. Choose the Right API

**Consensus Network API** (`http://localhost:1317`):
- Use for: Querying blockchain state, submitting transactions
- Best for: On-chain data, transaction operations
- Authentication: Transaction signing required for transactions

**Web Application API** (`http://localhost:8080`):
- Use for: Enhanced game state, player statistics, guild data
- Best for: Game-specific data, player operations
- Authentication: Session-based (optional for most endpoints)

**Streaming API** (GRASS/NATS):
- Use for: Real-time game state updates
- Best for: Monitoring, event-driven operations
- Authentication: Optional NATS authentication

### 2. Understand Base URLs

```json
{
  "consensusNetwork": {
    "baseUrl": "http://localhost:1317",
    "basePath": "/structs"
  },
  "webApplication": {
    "baseUrl": "http://localhost:8080",
    "basePath": "/api"
  },
  "streaming": {
    "natsProtocol": "nats://localhost:4222",
    "natsWebSocket": "ws://localhost:1443"
  }
}
```

---

## Common Scenarios

### Scenario 1: Get Player Information

**Goal**: Retrieve complete player information

**Approach**:
1. Use Web Application API for enhanced data
2. Use Consensus Network API for on-chain data
3. Combine both for complete picture

**Example Workflow**:
```json
{
  "steps": [
    {
      "step": 1,
      "api": "webapp",
      "endpoint": "webapp-player-by-id",
      "url": "/api/player/{player_id}"
    },
    {
      "step": 2,
      "api": "consensus",
      "endpoint": "player-by-id",
      "url": "/structs/player/{id}"
    },
    {
      "step": 3,
      "api": "webapp",
      "endpoint": "planet-by-player",
      "url": "/structs/planet_by_player/{playerId}"
    }
  ]
}
```

**See**: `examples/workflows/get-player-and-planets.json`

### Scenario 2: Monitor Planet Status

**Goal**: Monitor planet shield and raid status

**Approach**:
1. Query current state
2. Set up streaming subscription
3. React to events

**Example Workflow**:
```json
{
  "steps": [
    {
      "step": 1,
      "api": "webapp",
      "endpoint": "webapp-planet-by-id"
    },
    {
      "step": 2,
      "api": "webapp",
      "endpoint": "webapp-planet-shield-health"
    },
    {
      "step": 3,
      "api": "streaming",
      "subscription": "structs.planet.{planet_id}"
    }
  ]
}
```

**See**: `examples/workflows/monitor-planet-shield.json`

### Scenario 3: Query Guild Statistics

**Goal**: Get comprehensive guild statistics

**Approach**:
1. Get guild basic info
2. Get member count
3. Get power stats
4. Execute steps 2-3 in parallel

**Example Workflow**:
```json
{
  "steps": [
    {
      "step": 1,
      "api": "webapp",
      "endpoint": "webapp-guild-by-id"
    },
    {
      "step": 2,
      "api": "webapp",
      "endpoint": "webapp-guild-member-count",
      "parallel": true
    },
    {
      "step": 3,
      "api": "webapp",
      "endpoint": "webapp-guild-power-stats",
      "parallel": true
    }
  ]
}
```

**See**: `examples/workflows/query-guild-stats.json`

---

## Best Practices

### 1. Use Appropriate API for Task

**Web Application API**:
- ✅ Player statistics
- ✅ Guild data
- ✅ Planet shield information
- ✅ Ore statistics
- ✅ Enhanced game state

**Consensus Network API**:
- ✅ On-chain entity data
- ✅ Transaction submission
- ✅ Block height queries
- ✅ Module parameters

**Streaming API**:
- ✅ Real-time monitoring
- ✅ Event-driven operations
- ✅ State change notifications

### 2. Implement Error Handling

**Always**:
- Check response status codes
- Validate response format
- Handle retryable errors
- Log non-retryable errors

**Example**:
```json
{
  "errorHandling": {
    "retryable": [500, 503, "timeout", "network"],
    "nonRetryable": [400, 404, 401, 403],
    "strategy": "exponential-backoff"
  }
}
```

**See**: `protocols/error-handling.md`

### 3. Respect Rate Limits

**Best Practices**:
- Monitor rate limit headers
- Implement request throttling
- Use exponential backoff on 429 errors
- Cache responses when possible

**Example**:
```json
{
  "rateLimiting": {
    "monitorHeaders": true,
    "throttleRequests": true,
    "cacheResponses": true,
    "maxRequestsPerMinute": 60
  }
}
```

**See**: `api/rate-limits.yaml`

### 4. Use Pagination Correctly

**For Large Datasets**:
- Start with empty pagination key
- Use next_key for subsequent pages
- Continue until next_key is null
- Use appropriate page size (50-100)

**Example**:
```json
{
  "pagination": {
    "strategy": "key-based",
    "pageSize": 100,
    "continueUntil": "next_key is null"
  }
}
```

**See**: `patterns/pagination.md`

### 5. Cache Responses

**When to Cache**:
- Static or slowly-changing data
- Frequently accessed endpoints
- Expensive queries
- Rate-limited endpoints

**Cache Strategy**:
```json
{
  "caching": {
    "enabled": true,
    "ttl": 300,
    "keys": ["player-{id}", "guild-{id}", "planet-{id}"]
  }
}
```

### 6. Use Streaming for Real-time Data

**When to Use Streaming**:
- Monitoring entity state
- Reacting to events
- Real-time dashboards
- Event-driven workflows

**Example**:
```json
{
  "streaming": {
    "subjects": ["structs.player.*", "structs.planet.*"],
    "connection": "nats://localhost:4222",
    "reconnect": true
  }
}
```

**See**: `protocols/streaming.md`

---

## Performance Optimization

### 1. Parallel Requests

**When Possible**:
- Independent queries
- No data dependencies
- Multiple entity queries

**Example**:
```json
{
  "parallel": [
    "GET /api/guild/{id}/members/count",
    "GET /api/guild/{id}/power/stats"
  ]
}
```

### 2. Batch Operations

**When Supported**:
- Multiple entity queries
- Bulk operations
- Transaction batching

### 3. Reduce API Calls

**Strategies**:
- Cache responses
- Use streaming for updates
- Combine queries when possible
- Use appropriate page sizes

### 4. Optimize Queries

**Best Practices**:
- Use specific endpoints (by-id) when possible
- Filter at API level when supported
- Avoid unnecessary pagination
- Use appropriate query parameters

---

## Common Patterns

### Pattern 1: Query and Monitor

**Use Case**: Query current state, then monitor changes

**Pattern**:
1. Query current state
2. Subscribe to streaming events
3. Update state on events

**See**: `examples/workflows/query-and-monitor-planet.json`

### Pattern 2: Authenticated Operations

**Use Case**: Operations requiring authentication

**Pattern**:
1. Authenticate
2. Store session
3. Use session in requests
4. Handle session expiration

**See**: `examples/workflows/authenticated-guild-query.json`

### Pattern 3: Multi-Step Queries

**Use Case**: Chaining queries with dependencies

**Pattern**:
1. Query base entity
2. Extract IDs
3. Query related entities
4. Combine results

**See**: `examples/workflows/get-player-and-planets.json`

---

## Troubleshooting

### Common Issues

**1. 404 Not Found**:
- Verify entity ID format
- Check if entity exists
- Verify endpoint path

**2. 429 Rate Limit**:
- Reduce request frequency
- Implement throttling
- Respect Retry-After header

**3. 500 Server Error**:
- Retry with exponential backoff
- Check server status
- Log error details

**4. Authentication Errors**:
- Verify credentials
- Check session expiration
- Re-authenticate if needed

**5. Streaming Connection Issues**:
- Verify NATS server running
- Check connection URL
- Implement reconnection logic

---

## Testing Your Integration

### 1. Test All Endpoints

**Before Production**:
- Test each endpoint you plan to use
- Verify response formats
- Test error cases
- Validate schemas

**See**: `protocols/testing-protocol.md`

### 2. Test Error Handling

**Scenarios**:
- Network errors
- Server errors
- Rate limiting
- Authentication failures

### 3. Test Workflows

**Complete Flows**:
- Multi-step operations
- Authentication flows
- Streaming subscriptions
- Error recovery

---

## Related Documentation

**Protocols**:
- `protocols/query-protocol.md` - Query patterns
- `protocols/action-protocol.md` - Transaction patterns
- `protocols/webapp-api-protocol.md` - Webapp patterns
- `protocols/streaming.md` - Streaming patterns
- `protocols/authentication.md` - Authentication patterns
- `protocols/error-handling.md` - Error handling
- `protocols/testing-protocol.md` - Testing patterns

**Patterns**:
- `patterns/pagination.md` - Pagination patterns

**Examples**:
- `examples/workflows/` - Workflow examples
- `examples/errors/` - Error examples
- `examples/auth/` - Authentication examples

**Reference**:
- `reference/api-quick-reference.md` - Quick reference
- `reference/endpoint-quick-lookup.json` - Endpoint lookup
- `api/endpoints.yaml` - Complete endpoint catalog
- `api/error-codes.yaml` - Error codes
- `api/rate-limits.yaml` - Rate limits

---

## Quick Tips

1. **Start with Web Application API** for most game data
2. **Use Consensus Network API** for on-chain operations
3. **Use Streaming API** for real-time monitoring
4. **Always handle errors** gracefully
5. **Respect rate limits** and implement throttling
6. **Cache responses** when appropriate
7. **Test thoroughly** before production
8. **Monitor performance** and optimize

---

*API Documentation Specialist - December 7, 2025*

