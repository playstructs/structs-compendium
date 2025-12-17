# AI Agent Guide

**Version**: 1.0.0  
**Purpose**: Comprehensive guide for AI agents working with Structs  
**Last Updated**: December 7, 2025

---

## Overview

This guide provides AI agents with everything needed to effectively interact with Structs. It covers core concepts, common workflows, best practices, error handling, and performance optimization.

**This guide is designed as a starting point** that can be customized for your specific project needs.

---

## Table of Contents

1. [Core Concepts](#core-concepts)
2. [Getting Started](#getting-started)
3. [Common Workflows](#common-workflows)
4. [Best Practices](#best-practices)
5. [Error Handling](#error-handling)
6. [Performance Optimization](#performance-optimization)
7. [Quick References](#quick-references)
8. [Next Steps](#next-steps)

---

## Core Concepts

### Game Entities

Structs is built around several core entities:

- **Player**: The main entity representing a player account
- **Planet**: Planets that can be explored and developed
- **Struct**: Buildings and structures on planets
- **Guild**: Player organizations for coordination
- **Fleet**: Player's fleet of ships (including Command Ship)

**Entity IDs**: All entities use the format `type-index` (e.g., `1-11` for player type 1, index 11)

**See**: `schemas/entities.json` for complete entity definitions

### API Structure

Structs provides three main APIs:

1. **Consensus Network API** (`http://localhost:1317`)
   - Query game state
   - Submit transactions
   - On-chain operations

2. **Web Application API** (`http://localhost:8080`)
   - Enhanced game data
   - Player statistics
   - Guild information
   - Authentication

3. **Streaming API** (GRASS/NATS)
   - Real-time game state updates
   - Event notifications
   - WebSocket transport

**See**: `reference/api-quick-reference.md` for complete API reference

### Actions and Transactions

All game actions are submitted as transactions:

- **Build Struct**: Two-step process (initiate → complete)
- **Mine Resources**: Requires proof-of-work
- **Attack**: Combat actions
- **Explore**: Planet exploration

**Important**: Transaction status `broadcast` does NOT mean action succeeded! Always verify game state after transactions.

**See**: `reference/action-quick-reference.md` for complete action reference

---

## Getting Started

### Step 1: Load Documentation Efficiently

**Start with minimal load** to maximize context window:

1. Read `LOADING_STRATEGY.md` - Learn efficient loading strategies
2. Load indexes first: `reference/endpoint-index.json`, `reference/action-index.json`
3. Load only what you need for your current task

**See**: `LOADING_STRATEGY.md` for detailed loading strategies

### Step 2: Understand Game State

**Essential schemas**:
- `schemas/entities.json` - Entity definitions
- `schemas/game-state.json` - Complete game state structure
- `schemas/formats.json` - ID format specifications

**Minimal schemas** (for simple operations):
- `schemas/minimal/player-essential.json`
- `schemas/minimal/planet-essential.json`
- `schemas/minimal/struct-essential.json`

### Step 3: Learn Query Patterns

**Query protocols**:
- `protocols/query-protocol.md` - How to query game state
- `api/queries/{entity}.yaml` - Entity-specific query endpoints

**Common queries**:
- Get player: `GET /structs/player/{id}`
- Get planet: `GET /structs/planet/{id}`
- Get struct: `GET /structs/struct/{id}`

**See**: `reference/api-quick-reference.md` for common endpoints

### Step 4: Learn Action Patterns

**Action protocols**:
- `protocols/action-protocol.md` - How to perform actions
- `schemas/actions.json` - Action definitions

**Transaction flow**:
1. Get account info
2. Build transaction
3. Sign transaction
4. Submit transaction
5. Wait for confirmation
6. **Verify action occurred** (query game state)

**See**: `reference/action-quick-reference.md` for action patterns

### Step 5: Implement Error Handling

**Error handling**:
- `protocols/error-handling.md` - Error handling patterns
- `schemas/errors.json` - Error code definitions
- `patterns/retry-strategies.md` - Retry logic

**Key principles**:
- Always check response status
- Categorize errors (retryable vs non-retryable)
- Implement retry logic for transient errors
- Verify game state after actions

---

## Common Workflows

### Workflow 1: Query Player and Planets

**Pattern**: Linear Chain

**Steps**:
1. Get player by ID
2. Get player's planets
3. Process combined data

**Example**: See `examples/workflows/get-player-and-planets.json`

**Pattern**: `patterns/workflow-patterns.md#linear-chain`

### Workflow 2: Build a Struct

**Pattern**: Two-Step Action

**Steps**:
1. Check requirements (resources, power, location)
2. Initiate build (`struct-build-initiate`)
3. Wait for confirmation
4. Query struct state (wait for building state)
5. Complete build (`struct-build-complete`) with proof-of-work
6. Verify struct created

**See**: `reference/action-quick-reference.md#construction-actions`

### Workflow 3: Monitor Planet Shield

**Pattern**: Hybrid (Query + Streaming)

**Steps**:
1. Get planet information (query)
2. Subscribe to planet events (streaming)
3. Monitor shield health updates
4. Handle shield status changes

**Example**: See `examples/workflows/monitor-planet-shield.json`

**Pattern**: `patterns/polling-vs-streaming.md#hybrid-patterns`

### Workflow 4: Authenticated Guild Query

**Pattern**: Linear Chain with Authentication

**Steps**:
1. Login to webapp API
2. Store session cookie
3. Make authenticated request
4. Process guild data

**Example**: See `examples/workflows/authenticated-guild-query.json`

**See**: `protocols/authentication.md` for authentication details

---

## Best Practices

### 1. Use Patterns

**Always use established patterns** for common scenarios:

- **Pagination**: `patterns/pagination.md` - Handle paginated responses
- **Caching**: `patterns/caching.md` - Cache API responses
- **Rate Limiting**: `patterns/rate-limiting.md` - Handle rate limits
- **Retry Logic**: `patterns/retry-strategies.md` - Retry failed requests
- **Workflows**: `patterns/workflow-patterns.md` - Multi-step operations
- **Security**: `patterns/security.md` - Security best practices

**Quick lookup**: `patterns/QUICK_REFERENCE.md`

### 2. Implement Caching

**Cache responses** to reduce API calls:

- Static data: Long TTL (1+ hour)
- Slowly-changing data: Medium TTL (5 minutes)
- Frequently-changing data: Short TTL (10-30 seconds)

**See**: `patterns/caching.md` for caching strategies

### 3. Handle Rate Limits

**Monitor rate limits** and throttle requests:

- Consensus Network: 60 requests/minute
- Web Application: 100 requests/minute
- Use rate limit headers to track usage

**See**: `patterns/rate-limiting.md` for rate limit handling

### 4. Use Streaming for Real-time Data

**Choose streaming** for frequently-changing data:

- Planet shield health (during raids)
- Fleet movement status
- Resource amounts (during mining)

**See**: `patterns/polling-vs-streaming.md` for decision framework

### 5. Verify Actions

**Always verify** that actions actually occurred:

- Transaction status `broadcast` ≠ action succeeded
- Query game state after transactions
- Check requirements if action failed

**See**: `protocols/action-protocol.md#validation-warning`

### 6. Secure Credentials

**Store credentials securely**:

- Use environment variables
- Never log private keys
- Handle session expiration gracefully

**See**: `patterns/security.md` for security best practices

---

## Error Handling

### Error Categories

**Retryable Errors** (can be retried):
- `GENERAL_ERROR` (code: 1)
- `INTERNAL_SERVER_ERROR` (code: 500)
- `REQUEST_TIMEOUT`
- `NETWORK_ERROR`
- `RATE_LIMIT_EXCEEDED` (code: 429) - Retry after delay

**Non-Retryable Errors** (should not be retried):
- `INSUFFICIENT_FUNDS` (code: 2)
- `PLAYER_HALTED` (code: 6)
- `INSUFFICIENT_CHARGE` (code: 7)
- `INVALID_LOCATION` (code: 8)
- `ENTITY_NOT_FOUND` (code: 404)

**See**: `schemas/errors.json` for complete error catalog

### Retry Strategy

**Use exponential backoff** for retryable errors:

- Initial delay: 1 second
- Max retries: 3
- Backoff multiplier: 2
- Max delay: 10 seconds

**See**: `patterns/retry-strategies.md` for retry patterns

### Error Recovery

**Common recovery patterns**:

- **Player Halted**: Wait for player to come online
- **Insufficient Charge**: Wait for charge to accumulate
- **Rate Limited**: Wait for rate limit window to reset
- **Network Error**: Retry with exponential backoff

**See**: `protocols/error-handling.md` for error recovery patterns

---

## Performance Optimization

### 1. Use Caching

**Cache frequently accessed data**:

- Player information: 5 minutes TTL
- Planet information: 5 minutes TTL
- Struct types: 1 hour TTL (rarely changes)

**See**: `patterns/caching.md` for caching strategies

### 2. Use Parallel Requests

**Execute independent requests in parallel**:

- Get player, guild, and planet (if independent)
- Batch similar operations
- Use Promise.all() or equivalent

**See**: `patterns/workflow-patterns.md#parallel-patterns`

### 3. Use Streaming for Real-time Data

**Stream frequently-changing data** instead of polling:

- Reduces API calls
- Lower latency
- More efficient

**See**: `patterns/polling-vs-streaming.md` for decision framework

### 4. Monitor Rate Limits

**Track rate limit usage**:

- Monitor rate limit headers
- Throttle requests to stay within limits
- Use caching to reduce API calls

**See**: `patterns/rate-limiting.md` for rate limit monitoring

### 5. Optimize Loading Strategy

**Load documentation incrementally**:

- Start with indexes
- Load only what you need
- Use minimal schemas when possible

**See**: `LOADING_STRATEGY.md` for loading strategies

---

## Quick References

### API Endpoints

**Quick Reference**: `reference/api-quick-reference.md`

**Common Endpoints**:
- Player: `GET /structs/player/{id}`
- Planet: `GET /structs/planet/{id}`
- Struct: `GET /structs/struct/{id}`
- Submit Transaction: `POST /cosmos/tx/v1beta1/txs`

### Actions

**Quick Reference**: `reference/action-quick-reference.md`

**Common Actions**:
- Build Struct: `struct-build-initiate` → `struct-build-complete`
- Activate Struct: `struct-activate`
- Attack: `struct-attack`
- Explore Planet: `planet-explore`

### Patterns

**Quick Reference**: `patterns/QUICK_REFERENCE.md`

**Common Patterns**:
- Rate Limiting: `patterns/rate-limiting.md`
- Caching: `patterns/caching.md`
- Retry Strategies: `patterns/retry-strategies.md`
- Workflows: `patterns/workflow-patterns.md`
- Security: `patterns/security.md`

### Workflows

**Examples**: `examples/workflows/README.md`

**Common Workflows**:
- Get Player and Planets: `examples/workflows/get-player-and-planets.json`
- Monitor Planet Shield: `examples/workflows/monitor-planet-shield.json`
- Authenticated Guild Query: `examples/workflows/authenticated-guild-query.json`

---

## Implementation Checklist

**For a step-by-step implementation guide**, see `IMPLEMENTATION_CHECKLIST.md`:
- Phase-by-phase implementation
- Essential reading order
- Common pitfalls to avoid
- Success criteria

---

## Next Steps

### 1. Explore Documentation

**Start with**:
- `LOADING_STRATEGY.md` - Efficient documentation loading
- `reference/api-quick-reference.md` - API quick reference
- `reference/action-quick-reference.md` - Action quick reference
- `patterns/QUICK_REFERENCE.md` - Pattern quick reference

### 2. Review Examples

**Study examples**:
- `examples/simple-bot.json` - Simple bot implementation
- `examples/workflows/` - Workflow examples
- `examples/errors/` - Error handling examples

### 3. Implement Patterns

**Use patterns** for common scenarios:
- Rate limiting
- Caching
- Retry logic
- Workflows
- Security

### 4. Test Your Implementation

**Test thoroughly**:
- Test with real API
- Handle edge cases
- Monitor performance
- Check error handling

### 5. Optimize Performance

**Optimize**:
- Implement caching
- Use parallel requests
- Choose streaming vs polling
- Monitor rate limits

---

## Customization

This guide is designed as a **starting point** that can be customized for your specific project needs.

### Customize for Your Project

**Add project-specific**:
- Custom workflows
- Project-specific patterns
- Domain-specific examples
- Integration details

### Extend the Guide

**Extend with**:
- Additional workflows
- Custom patterns
- Project-specific best practices
- Integration guides

---

## Related Documentation

**Protocols**:
- `protocols/query-protocol.md` - Query patterns
- `protocols/action-protocol.md` - Action patterns
- `protocols/error-handling.md` - Error handling
- `protocols/authentication.md` - Authentication

**Schemas**:
- `schemas/entities.json` - Entity definitions
- `schemas/actions.json` - Action definitions
- `schemas/errors.json` - Error codes

**Patterns**:
- `patterns/README.md` - Pattern documentation index
- `patterns/QUICK_REFERENCE.md` - Pattern quick reference

**Examples**:
- `examples/workflows/README.md` - Workflow examples
- `examples/errors/` - Error examples
- `examples/auth/` - Authentication examples

**Reference**:
- `reference/api-quick-reference.md` - API quick reference
- `reference/action-quick-reference.md` - Action quick reference
- `reference/endpoint-index.json` - Endpoint index
- `reference/action-index.json` - Action index

---

## Support

**Documentation Issues**:
- Check `troubleshooting/` for common issues
- Review error examples in `examples/errors/`
- Check `api/error-codes.yaml` for error codes

**Questions**:
- Review relevant protocol documentation
- Check pattern documentation
- Review workflow examples

---

*AI Agent Guide Version: 1.0.0 - December 7, 2025*

*This guide is designed as a starting point and can be customized for your specific project needs.*
