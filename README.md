# Structs Compendium - AI Agent Documentation

**Purpose**: Machine-readable documentation for AI agents, bots, and automated systems interacting with Structs.

**Format**: All documentation is structured for AI consumption - JSON schemas, structured data, clear patterns.

## Structs
In the distant future the species of the galaxy are embroiled in a race for Alpha Matter, the rare and dangerous substance that fuels galactic civilization. Players take command of Structs, a race of sentient machines, and must forge alliances, conquer enemies and expand their influence to control Alpha Matter and the fate of the galaxy.

---

## Directory Structure

```
├── README.md                    # This file
├── AGENTS.md                    # 🤖 AI Agent Guide: Comprehensive guide for AI agents
├── LOADING_STRATEGY.md          # 🔴 READ FIRST: How to efficiently load documentation
├── DOCUMENTATION_INDEX.md       # 📚 Complete documentation index
├── IMPLEMENTATION_CHECKLIST.md  # ✅ Step-by-step implementation checklist
├── BEST_PRACTICES.md            # ⭐ Best practices summary
├── schemas/                     # Data structure definitions
│   ├── game-state.json         # Complete game state schema (reference)
│   ├── minimal/                # ✅ Minimal schemas for simple operations
│   │   ├── player-essential.json
│   │   ├── planet-essential.json
│   │   └── struct-essential.json
│   ├── entities/               # ✅ Individual entity schemas (split for efficiency)
│   │   ├── player.json
│   │   ├── planet.json
│   │   ├── struct.json
│   │   └── ... (9 entity files)
│   ├── entities.json           # Entity metadata and relationships
│   ├── actions.json            # Action/command definitions
│   ├── gameplay.json           # Gameplay mechanics schema
│   ├── responses.json          # API response schemas
│   └── errors.json             # Error code definitions
├── api/                        # API specifications
│   ├── endpoints.yaml          # Complete endpoint catalog (reference)
│   ├── queries/                # ✅ Individual query endpoint files (split for efficiency)
│   │   ├── player.yaml
│   │   ├── planet.yaml
│   │   └── ... (13 query files)
│   ├── transactions/           # ✅ Transaction endpoint files
│   │   └── submit-transaction.yaml
│   └── streaming/              # GRASS streaming protocol
├── protocols/                  # Communication protocols
│   ├── query-protocol.md       # How to query game state
│   ├── action-protocol.md      # How to perform actions
│   ├── gameplay-protocol.md    # How to interact with gameplay mechanics
│   ├── error-handling.md       # Error handling patterns
│   └── authentication.md       # Authentication patterns
├── patterns/                   # Common patterns and best practices
│   ├── README.md              # Pattern documentation index
│   ├── QUICK_REFERENCE.md     # Quick pattern lookup guide
│   ├── pagination.md           # Pagination patterns
│   ├── rate-limiting.md       # Rate limiting strategies
│   ├── caching.md             # Caching strategies
│   ├── polling-vs-streaming.md # When to use each
│   ├── retry-strategies.md    # Retry logic patterns
│   ├── workflow-patterns.md   # Multi-step workflow patterns
│   ├── security.md            # Security best practices
│   ├── state-sync.md          # Keeping state synchronized
│   └── gameplay-strategies.md # Gameplay strategy patterns
├── examples/                   # Working examples
│   ├── simple-bot.json         # Simple bot implementation
│   ├── state-monitor.json      # State monitoring example
│   ├── action-executor.json    # Action execution example
│   ├── gameplay-mining-bot.json # Mining bot example
│   ├── gameplay-combat-bot.json # Combat bot example
│   ├── workflows/             # Workflow examples
│   │   ├── README.md          # Workflow examples index
│   │   └── *.json            # Individual workflow examples
│   ├── errors/                # Error examples
│   └── auth/                  # Authentication examples
└── reference/                  # Quick reference
    ├── endpoint-index.json     # All endpoints indexed
    ├── entity-index.json       # All entities indexed
    ├── action-index.json       # All actions indexed
    ├── api-quick-reference.md  # API quick reference guide
    └── action-quick-reference.md # Action quick reference guide
├── visuals/                    # Visual content (graphs, spatial data)
    ├── README.md              # Visual content overview
    ├── schemas/               # Visual schema definitions (7 schemas)
    ├── graphs/                # Machine-readable graph data (4 graphs)
    ├── spatial/               # Spatial/geometric data
    └── reference/             # Visual content index
```

---

## Documentation Standards

### Format Conventions

1. **JSON Schemas**: All data structures defined in JSON Schema format
2. **YAML for APIs**: API specifications in YAML for readability
3. **Structured Markdown**: Markdown with clear structure and machine-parseable sections
4. **Code Examples**: All examples in JSON format (not code snippets)

### Naming Conventions

- **Schemas**: `{entity}.json` (e.g., `player.json`, `planet.json`)
- **Protocols**: `{protocol-name}.md` (e.g., `query-protocol.md`)
- **Examples**: `{use-case}.json` (e.g., `simple-bot.json`)

### Versioning

- All schemas include `$schema` and `version` fields
- Protocol versions tracked in protocol files
- Breaking changes documented in `CHANGELOG.md`

---

## Quick Start for AI Agents

### 0. Agent Guide (START HERE!)
- **Read `AGENTS.md` first** - Comprehensive guide for AI agents working with Structs
- **Covers**: Core concepts, workflows, best practices, error handling, performance optimization
- **Customizable**: Designed as a starting point that can be adapted for your project

### 1. Loading Strategy
- **Read `LOADING_STRATEGY.md`** - Learn how to efficiently load documentation
- **Start with minimal load** - Use indexes and minimal schemas
- **Load incrementally** - Only load what you need for your task

### 1. Understand Game State
- Read `schemas/game-state.json` for complete state structure
- Read `schemas/entities.json` for entity definitions
- Read `schemas/economics.json` for economic entities and formulas

### 2. Learn to Query
- Read `protocols/query-protocol.md` for query patterns
- **For entity queries**: Use `api/queries/{entity}.yaml` (30-60 lines each)
- **For all queries**: See `api/endpoints.yaml` (1180 lines - reference only)
- Review `reference/endpoint-index.json` to find endpoints

### 3. Learn to Act
- Read `protocols/action-protocol.md` for action patterns
- Read `protocols/economic-protocol.md` for economic operations
- Review `api/transactions/submit-transaction.yaml` (and `api/transactions/README.md`) for transaction examples

### 4. Handle Errors
- Read `protocols/error-handling.md` for error patterns
- Review `schemas/errors.json` for error codes

### 5. Understand Gameplay
- Read `schemas/gameplay.json` for gameplay mechanics
- Read `protocols/gameplay-protocol.md` for gameplay interactions
- Review `examples/gameplay-mining-bot.json` for mining workflows
- Review `examples/gameplay-combat-bot.json` for combat workflows

### 6. Optimize Performance
- Read `patterns/state-sync.md` for state synchronization
- Read `patterns/polling-vs-streaming.md` for real-time updates
- Read `patterns/gameplay-strategies.md` for gameplay optimization
- Read `patterns/caching.md` for response caching
- Read `patterns/rate-limiting.md` for rate limit handling
- Read `patterns/retry-strategies.md` for retry logic

### 7. Economic Operations
- Read `schemas/economics.json` for economic entities and formulas
- Read `protocols/economic-protocol.md` for economic operation patterns
- Review `examples/economic-bot.json` for economic bot implementation

### 8. Implement Best Practices
- Read `patterns/security.md` for security best practices
- Read `patterns/workflow-patterns.md` for multi-step operations
- Review `patterns/QUICK_REFERENCE.md` for pattern selection
- Review `reference/api-quick-reference.md` for API quick reference
- Review `reference/action-quick-reference.md` for action quick reference

---

## ID Format Specification

**Object IDs in Structs follow the format `#-#` where:**
- **First number**: Object type identifier (0-11)
- **Second number**: Object index (1-based)

**Object Type Identifiers**:
- `0` = Guild
- `1` = Player
- `2` = Planet
- `3` = Reactor
- `4` = Substation
- `5` = Struct
- `6` = Allocation
- `7` = Infusion
- `8` = Address
- `9` = Fleet
- `10` = Provider
- `11` = Agreement

**Examples**:
- `0-1` = First guild (guild type 0, index 1)
- `1-11` = Eleventh player (player type 1, index 11)
- `2-1` = First planet (planet type 2, index 1)
- `4-3` = Third substation (substation type 4, index 3)

**Exception**: `struct_types` use regular integer IDs (e.g., `1`, `2`, `3`) without the `#-#` format.

**When using IDs in API requests or schemas, always use the `#-#` format for entity IDs.**

---

## Schema Language

All schemas use JSON Schema Draft 7 with Structs-specific extensions:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "version": "1.0.0",
  "structs:entity": "Player",
  "structs:category": "core",
  "structs:endpoint": "/structs/player/{id}",
  "description": "Player entity definition",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "structs:format": "entity-id",
      "pattern": "^[0-9]+-[0-9]+$",
      "description": "Unique player identifier in format 'type-index' (e.g., '1-11' for player type 1, index 11)"
    }
  }
}
```

### Structs-Specific Schema Extensions

- `structs:entity`: Entity type name
- `structs:category`: Entity category (core, resource, economic, etc.)
- `structs:endpoint`: Primary API endpoint
- `structs:format`: Data format specification (use "entity-id" for `#-#` format IDs)
- `structs:required-for`: What this entity is required for
- `structs:relationships`: Relationships to other entities

### ID Format in Schemas

When documenting entity IDs in JSON schemas:
- Use `"structs:format": "entity-id"` for `#-#` format IDs
- Use `"pattern": "^[0-9]+-[0-9]+$"` to validate the format
- Include description explaining the format: "Entity identifier in format 'type-index'"
- For struct_types, use `"type": "integer"` instead

---

## Protocol Language

Protocols are defined in structured markdown with clear sections:

```markdown
# Protocol Name

**Version**: 1.0.0
**Category**: Query | Action | Error | Authentication
**Status**: Stable | Experimental | Deprecated

## Overview
[Clear description]

## Request Format
[Structured request format]

## Response Format
[Structured response format]

## Examples
[JSON examples]

## Error Cases
[Error handling]

## Best Practices
[Recommendations]
```

---

## Index Files

All index files use consistent structure:

```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-12-07",
  "items": [
    {
      "id": "unique-id",
      "name": "Display Name",
      "category": "category",
      "endpoint": "/path/to/endpoint",
      "schema": "schemas/entity.json",
      "protocol": "protocols/protocol.md"
    }
  ]
}
```

---

## Versioning

- **Major versions**: Breaking changes to schemas or protocols
- **Minor versions**: New features, non-breaking changes
- **Patch versions**: Bug fixes, clarifications

---

## Contributing

When adding new documentation:

1. Follow schema format conventions
2. Include examples in JSON format
3. Update relevant index files
4Ensure machine-parseable structure

---

# Learn more

- [Structs](https://playstructs.com)
- [Project Wiki](https://watt.wiki)
- [@PlayStructs Twitter](https://twitter.com/playstructs)


# License

Copyright 2025 [Slow Ninja Inc](https://slow.ninja).

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

