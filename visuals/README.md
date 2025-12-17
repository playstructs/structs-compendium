# Visual Content

**Version**: 1.0.0  
**Category**: Visual  
**Status**: 🟡 In Progress

## Overview

This directory contains machine-readable visual content for AI agents. Visual content includes diagrams, graphs, spatial data, image metadata, UI elements, and visual patterns.

---

## Directory Structure

### Schemas

- **`schemas/`** - Visual schema definitions
  - `diagram-schema.json` - Schema for diagram structures
  - `relationship-graph.json` - Schema for relationship graphs
  - `flow-schema.json` - Schema for process flows
  - `decision-tree.json` - Schema for decision trees
  - `spatial-schema.json` - Schema for spatial/geometric data
  - `ui-schema.json` - Schema for UI elements
  - `pattern-schema.json` - Schema for visual patterns

### Graphs

- **`graphs/`** - Machine-readable graph data
  - `resource-flow.json` - Resource conversion flow
  - `gameplay-economics.json` - Gameplay ↔ Economics relationships
  - `system-integration.json` - Complete system integration graph
  - `entity-relationships.json` - Entity relationship graph

### Spatial

- **`spatial/`** - Spatial/geometric data
  - `coordinate-system.json` - Coordinate system definition
  - `planet-positions.json` - Planet coordinate data
  - `fleet-locations.json` - Fleet positioning data

### Metadata

- **`metadata/`** - Image metadata
  - `screenshots.json` - Screenshot metadata
  - `diagrams.json` - Diagram metadata

### UI

- **`ui/`** - UI element documentation
  - `screens.json` - Screen definitions
  - `elements.json` - UI element definitions

### Patterns

- **`patterns/`** - Visual pattern library
  - `visual-patterns.json` - Common visual patterns

### Reference

- **`reference/`** - Visual content index
  - `visual-index.json` - All visual content indexed

---

## Visual Content Types

### Diagrams

**Purpose**: Represent processes, flows, and relationships

**Format**: JSON graph structure (see `schemas/diagram-schema.json`)

**Examples**:
- Resource conversion flow
- System integration diagrams
- Entity relationship graphs
- Process flows

---

### Spatial Data

**Purpose**: Represent geometric and positional data

**Format**: JSON coordinate system (see `schemas/spatial-schema.json`)

**Examples**:
- Planet positions
- Fleet locations
- Structure placement

---

### Image Metadata

**Purpose**: Provide structured metadata about images

**Format**: JSON Schema (see `schemas/ui-schema.json`)

**Examples**:
- Screenshot metadata
- Diagram metadata
- UI element locations

---

### UI Elements

**Purpose**: Document UI elements and their relationships

**Format**: JSON Schema (see `schemas/ui-schema.json`)

**Examples**:
- Screen definitions
- UI element definitions
- UI element relationships

---

## Integration with Other Documentation

### Links to Schemas

Visual content links to:
- Entity schemas (`/ai/schemas/entities.json`)
- Action schemas (`/ai/schemas/actions.json`)
- API endpoints (`/ai/api/endpoints.yaml`)

### Cross-References

Visual content includes:
- `relatedSchemas`: Links to relevant schemas
- `relatedEndpoints`: Links to relevant API endpoints
- `relatedDiagrams`: Links to related visual content

---

## Using Visual Content

### For AI Agents

1. **Load Visual Schema**: Understand visual data structure
2. **Query Visual Data**: Access graph, spatial, or metadata
3. **Link to Entities**: Connect visual data to entity schemas
4. **Use for Context**: Use visual data to understand relationships

### For Developers

1. **Reference Visual Schemas**: Use schemas to create visual content
2. **Update Visual Data**: Keep visual data current
3. **Link Visual Content**: Connect visual content to other documentation

---

## Status

**Current Status**: 🟡 In Progress

**Completed**:
- Directory structure created
- README documentation
- All visual schemas (7 schemas)
- Resource flow graph
- Entity relationships graph
- Gameplay ↔ Economics relationships graph
- System integration graph
- Coordinate system definition

**Pending**:
- Planet positions data
- Fleet locations data
- Image metadata (screenshots, diagrams)
- UI element documentation (screens, elements)
- Visual pattern library

---

## Related Documentation

- **Schemas**: `../schemas/` - Entity and action schemas
- **API**: `../api/` - API endpoint documentation
- **Protocols**: `../protocols/` - Protocol documentation
- **Systems**: `../systems/` - System documentation

---

*Last Updated: December 7, 2025*  
*Owner: Visual Designer*

