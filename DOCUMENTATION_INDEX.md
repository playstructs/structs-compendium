# Documentation Index

**Version**: 1.1.0  
**Last Updated**: January 1, 2026  
**Purpose**: Complete index of all AI-first documentation for easy discovery

---

## Overview

This index provides a comprehensive catalog of all AI-first documentation, organized by category and use case. Use this to quickly find the documentation you need.

---

## Getting Started

### For New AI Agents

1. **Start Here**: `AGENTS.md` - Comprehensive guide for AI agents
2. **Loading Strategy**: `LOADING_STRATEGY.md` - Efficient documentation loading
3. **Main README**: `README.md` - Documentation structure and conventions

---

## Documentation by Category

### Core Guides

- **`AGENTS.md`** - Comprehensive AI agent guide
  - Core concepts, workflows, best practices
  - Error handling, performance optimization
  - Quick references and next steps

- **`LOADING_STRATEGY.md`** - Efficient documentation loading
  - Loading tiers (minimal to complete)
  - Task-based loading patterns
  - Context window optimization

- **`IMPLEMENTATION_CHECKLIST.md`** - Step-by-step implementation checklist
  - Phase-by-phase implementation guide
  - Essential reading order
  - Common pitfalls to avoid
  - Success criteria

- **`BEST_PRACTICES.md`** - Best practices summary
  - Core principles
  - Quick reference
  - Common pitfalls
  - Implementation checklist

- **`README.md`** - Documentation structure
  - Directory structure
  - Format conventions
  - Quick start guide

---

## Protocols

**Location**: `protocols/`

### Core Protocols

1. **`query-protocol.md`** - How to query game state
   - Single entity queries
   - List queries
   - Filtered queries
   - Pagination

2. **`action-protocol.md`** - How to perform actions
   - Transaction flow
   - Signing transactions
   - Proof-of-work
   - Validation

3. **`error-handling.md`** - Error handling patterns
   - Error response formats
   - Retry strategies
   - Error recovery
   - Best practices

4. **`authentication.md`** - Authentication patterns
   - Webapp authentication
   - Transaction signing
   - NATS authentication
   - Session management

5. **`streaming.md`** - GRASS/NATS streaming
   - Connection setup
   - Subscription patterns
   - Event types
   - Best practices

6. **`webapp-api-protocol.md`** - Web Application API
   - Webapp-specific patterns
   - Enhanced data queries
   - Authentication flows

7. **`gameplay-protocol.md`** - Gameplay interactions
   - Gameplay mechanics
   - Resource management
   - Combat operations

8. **`economic-protocol.md`** - Economic operations
   - Trading patterns
   - Energy agreements
   - Market operations

9. **`testing-protocol.md`** - Testing patterns
   - Test strategies
   - Validation methods
   - Best practices

---

## Patterns

**Location**: `patterns/`

### Data Retrieval Patterns

- **`pagination.md`** - Pagination patterns
  - Key-based pagination
  - Offset-based pagination
  - Edge cases

- **`caching.md`** - Caching strategies
  - TTL-based caching
  - Event-based invalidation
  - Cache key patterns

- **`polling-vs-streaming.md`** - Data access methods
  - Decision framework
  - Comparison matrix
  - Hybrid approaches

### Error Handling Patterns

- **`rate-limiting.md`** - Rate limit handling
  - Detection strategies
  - Throttling patterns
  - 429 error handling

- **`retry-strategies.md`** - Retry logic
  - Exponential backoff
  - Circuit breakers
  - Error-specific strategies

### Workflow Patterns

- **`workflow-patterns.md`** - Multi-step operations
  - Sequential workflows
  - Parallel workflows
  - State management

### Security Patterns

- **`security.md`** - Security best practices
  - Credential management
  - Session security
  - Private key security

### Other Patterns

- **`state-sync.md`** - State synchronization
- **`gameplay-strategies.md`** - Gameplay optimization
- **`validation-patterns.md`** - Transaction validation

### Pattern Quick Reference

- **`QUICK_REFERENCE.md`** - Quick pattern lookup
- **`README.md`** - Pattern documentation index

---

## Schemas

**Location**: `schemas/`

### Core Schemas

- **`entities.json`** - Entity definitions
- **`actions.json`** - Action definitions
- **`game-state.json`** - Complete game state
- **`gameplay.json`** - Gameplay mechanics
- **`economics.json`** - Economic entities
- **`responses.json`** - API response schemas
- **`requests.json`** - API request schemas
- **`errors.json`** - Error code definitions
- **`formats.json`** - Format specifications
- **`database-schema.json`** - PostgreSQL database schema documentation (v0.8.0-beta)

### Minimal Schemas

**Location**: `schemas/minimal/`

- **`player-essential.json`** - Essential player info
- **`planet-essential.json`** - Essential planet info
- **`struct-essential.json`** - Essential struct info

### Entity Schemas

**Location**: `schemas/entities/`

Individual entity schemas (split for efficiency):
- `player.json`
- `planet.json`
- `struct.json`
- `guild.json`
- `fleet.json`
- And more...

---

## API Documentation

**Location**: `api/`

### Endpoint Catalogs

- **`endpoints.yaml`** - Complete endpoint catalog (reference)
- **`endpoints-by-entity.yaml`** - Entity-based organization
- **`error-codes.yaml`** - Error code catalog
- **`rate-limits.yaml`** - Rate limiting information

### Query Endpoints

**Location**: `api/queries/`

Individual query endpoint files:
- `player.yaml`
- `planet.yaml`
- `struct.yaml`
- `guild.yaml`
- And more...

### Transaction Endpoints

**Location**: `api/transactions/`

- **`submit-transaction.yaml`** - Transaction submission

### Streaming Documentation

**Location**: `api/streaming/`

- **`event-schemas.json`** - Event payload schemas
- **`event-types.yaml`** - Event type catalog
- **`subscription-patterns.yaml`** - Subscription patterns
- **`README.md`** - Streaming overview

### Cosmetic Mods API

- **`cosmetic-mods.yaml`** - Cosmetic mod API endpoints

---

## Reference Guides

**Location**: `reference/`

### Quick References

- **`api-quick-reference.md`** - API quick reference
  - Common endpoints
  - Response formats
  - Error handling
  - Authentication

- **`action-quick-reference.md`** - Action quick reference
  - Action categories
  - Common requirements
  - Action patterns
  - Transaction flow

### Indexes

- **`endpoint-index.json`** - All endpoints indexed
- **`action-index.json`** - All actions indexed
- **`entity-index.json`** - All entities indexed
- **`endpoint-quick-lookup.json`** - Quick endpoint lookup

---

## Examples

**Location**: `examples/`

### Bot Examples

- **`simple-bot.json`** - Simple bot implementation
- **`gameplay-mining-bot.json`** - Mining bot
- **`gameplay-combat-bot.json`** - Combat bot
- **`economic-bot.json`** - Economic bot
- **`working-api-examples.json`** - Working API examples

### Workflow Examples

**Location**: `examples/workflows/`

- **`README.md`** - Workflow examples index
- **`get-player-and-planets.json`** - Player/planet workflow
- **`monitor-planet-shield.json`** - Shield monitoring
- **`query-guild-stats.json`** - Guild stats workflow
- **`authenticated-guild-query.json`** - Authenticated query
- **`query-and-monitor-planet.json`** - Query + streaming
- **`install-and-use-cosmetic-mod.json`** - Cosmetic mod workflow

### Error Examples

**Location**: `examples/errors/`

- **`404-not-found.json`** - 404 error example
- **`429-rate-limit.json`** - Rate limit error
- **`500-server-error.json`** - Server error
- **`cosmetic-mod-invalid.json`** - Mod validation error
- **`cosmetic-mod-conflict.json`** - Mod conflict error

### Authentication Examples

**Location**: `examples/auth/`

- **`webapp-login.json`** - Webapp login
- **`consensus-transaction-signing.json`** - Transaction signing
- **`nats-connection.json`** - NATS connection

### Cosmetic Mod Examples

**Location**: `examples/cosmetic-mods/`

- **`simple-miner-mod.json`** - Simple mod example
- **`multi-language-mod.json`** - Multi-language mod
- **`guild-alpha-complete-mod.json`** - Complete mod example

---

## Guides

**Location**: `guides/`

- **`api-usage-guide.md`** - General API usage guide
  - API overview
  - Common patterns
  - Best practices
  - Troubleshooting

---

## Configuration

**Location**: `config/`

- **`networks.json`** - Network configurations
- **`environments.json`** - Environment configs

---

## Troubleshooting

**Location**: `troubleshooting/`

- **`README.md`** - Troubleshooting overview
- **`common-issues.md`** - Common issues and solutions
- **`error-codes.md`** - Error code reference

---

## Systems

**Location**: `systems/`

- **`README.md`** - Systems overview
- **`power-system.md`** - Power/energy system
- **`resource-system.md`** - Resource system
- **`fleet-system.md`** - Fleet system

---

## Lifecycles

**Location**: `lifecycles/`

- **`README.md`** - Lifecycles overview
- **`struct-lifecycle.md`** - Struct lifecycle
- **`planet-lifecycle.md`** - Planet lifecycle
- **`fleet-lifecycle.md`** - Fleet lifecycle
- **`player-lifecycle.md`** - Player lifecycle

---

## Visual Content

**Location**: `visuals/`

- **`README.md`** - Visual content overview
- **`reference/visual-index.json`** - Visual content index

### Visual Schemas

- **`schemas/diagram-schema.json`** - Diagram structures
- **`schemas/relationship-graph.json`** - Relationship graphs
- **`schemas/flow-schema.json`** - Process flows
- **`schemas/decision-tree.json`** - Decision trees
- **`schemas/pattern-schema.json`** - Visual patterns
- **`schemas/spatial-schema.json`** - Spatial/geometric data
- **`schemas/ui-schema.json`** - UI elements

### Visual Graphs

- **`graphs/resource-flow.json`** - Resource conversion flow
- **`graphs/entity-relationships.json`** - Entity relationship graph
- **`graphs/gameplay-economics.json`** - Gameplay ↔ Economics relationships
- **`graphs/system-integration.json`** - System integration graph

### Spatial Data

- **`spatial/coordinate-system.json`** - Galactic coordinate system

---

## Dependencies

**Location**: `dependencies/`

- **`README.md`** - Dependencies overview
- **`command-ship.json`** - Command Ship dependencies
- **`building.json`** - Building dependencies
- **`exploration.json`** - Exploration dependencies

---

## Documentation by Use Case

### I Want to Query Game State

1. Read `protocols/query-protocol.md`
2. Check `reference/api-quick-reference.md` for endpoints
3. Use `api/queries/{entity}.yaml` for entity-specific queries
4. Review `examples/workflows/get-player-and-planets.json`

### I Want to Perform Actions

1. Read `protocols/action-protocol.md`
2. Check `reference/action-quick-reference.md` for actions
3. Review `schemas/actions.json` for action definitions
4. See `examples/workflows/` for workflow examples

### I Want to Handle Errors

1. Read `protocols/error-handling.md`
2. Check `patterns/retry-strategies.md` for retry logic
3. Review `api/error-codes.yaml` for error codes
4. See `examples/errors/` for error examples

### I Want to Optimize Performance

1. Read `patterns/caching.md` for caching
2. Read `patterns/rate-limiting.md` for rate limits
3. Read `patterns/polling-vs-streaming.md` for data access
4. Read `LOADING_STRATEGY.md` for loading optimization

### I Want to Implement Security

1. Read `patterns/security.md` for security best practices
2. Read `protocols/authentication.md` for authentication
3. Review `examples/auth/` for authentication examples

### I Want to Build a Bot

1. Start with `AGENTS.md` - Comprehensive guide
2. Follow `IMPLEMENTATION_CHECKLIST.md` - Step-by-step checklist
3. Review `examples/simple-bot.json` - Simple bot
4. Check `examples/gameplay-mining-bot.json` - Mining bot
5. Review `patterns/workflow-patterns.md` - Workflow patterns

### I Want to Use Streaming

1. Read `protocols/streaming.md` - Streaming protocol
2. Check `api/streaming/` - Streaming documentation
3. Review `patterns/polling-vs-streaming.md` - Decision guide
4. See `examples/workflows/monitor-planet-shield.json` - Example

---

## Quick Lookup

### By File Type

**Protocols**: `protocols/*.md`  
**Patterns**: `patterns/*.md`  
**Schemas**: `schemas/*.json`  
**API Docs**: `api/*.yaml`  
**Examples**: `examples/**/*.json`  
**References**: `reference/*.md` or `reference/*.json`

### By Topic

**Querying**: `protocols/query-protocol.md`, `api/queries/`  
**Actions**: `protocols/action-protocol.md`, `schemas/actions.json`  
**Errors**: `protocols/error-handling.md`, `api/error-codes.yaml`  
**Streaming**: `protocols/streaming.md`, `api/streaming/`  
**Authentication**: `protocols/authentication.md`, `examples/auth/`  
**Workflows**: `patterns/workflow-patterns.md`, `examples/workflows/`  
**Caching**: `patterns/caching.md`  
**Rate Limiting**: `patterns/rate-limiting.md`  
**Security**: `patterns/security.md`  
**Visual Content**: `visuals/README.md`, `visuals/graphs/`, `visuals/reference/visual-index.json`

---

## Related Documentation

**Human-Readable Docs**: See root `README.md` for human-readable documentation

**Technical Docs**: See `technical/` for technical architecture and codebase documentation

**Gameplay Docs**: See `gameplay/` for gameplay guides and strategies

---

*Documentation Index Version: 1.1.0 - January 1, 2026*

