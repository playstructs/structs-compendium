# Review Documentation

**Purpose**: Documentation for reviewing external repositories for v0.8.0-beta changes  
**Last Updated**: January 1, 2026

---

## Overview

This directory contains review documentation for external Structs repositories. Each repository review includes checklists, guides, and workflows to systematically review code changes and update documentation.

---

## structs-webapp Review

**Repository**: [https://github.com/playstructs/structs-webapp/](https://github.com/playstructs/structs-webapp/)  
**Status**: Documentation Ready, Code Review Pending  
**Priority**: High

### Documentation Files

1. **`webapp-review-workflow.md`** ⭐ **START HERE**
   - Step-by-step workflow for completing the review
   - Commands and checklists
   - Documentation update procedures

2. **`webapp-clone-instructions.md`**
   - How to clone the repository safely
   - Git setup and verification

3. **`webapp-symfony-review-guide.md`**
   - Symfony-specific code review guide
   - File locations and patterns
   - What to look for in the codebase

4. **`webapp-v0.8.0-beta-review.md`**
   - Comprehensive review checklist
   - All items to verify
   - Expected impacts

5. **`webapp-review-summary.md`**
   - Summary of completed work
   - Status tracking

### Quick Start

```bash
# 1. Clone repository (safe - in .gitignore)
git clone https://github.com/playstructs/structs-webapp.git

# 2. Follow the workflow
# See: reviews/webapp-review-workflow.md
```

---

## Review Process

### Phase 1: Setup
1. Clone repository (already in `.gitignore`)
2. Check git history for v0.8.0-beta changes
3. Review project structure

### Phase 2: Code Review
1. Review API controllers
2. Check entity changes
3. Review database migrations
4. Verify response schemas

### Phase 3: Documentation Updates
1. Update endpoint documentation
2. Update response schemas
3. Document breaking changes
4. Add examples if needed

### Phase 4: Verification
1. Verify all endpoints documented
2. Confirm response schemas match
3. Check for breaking changes
4. Final review

---

## Related Documentation

- **Main Protocol**: `../protocols/webapp-api-protocol.md`
- **API Endpoints**: `../api/webapp/`
- **Entity Schemas**: `../schemas/entities/`
- **CHANGELOG**: `../CHANGELOG.md`

---

## structs-mcp Review

**Repository**: [https://github.com/playstructs/structs-mcp/](https://github.com/playstructs/structs-mcp/)  
**Status**: Code Review Complete, Findings Documented  
**Priority**: High

### Documentation Files

1. **`mcp-review-findings.md`** ⭐ **REVIEW FINDINGS**
   - Complete code review findings
   - Identified missing reactor staking actions
   - Query tool verification status

2. **`mcp-v0.8.0-beta-review.md`**
   - Review checklist
   - Items to verify

### Key Findings

- ✅ **Reactor Query Tools**: Exist and should return staking data
- ❌ **Reactor Staking Actions**: NOT implemented - need to be added
- ✅ **Query Tools**: Pass through all API fields (should include v0.8.0-beta fields)

### Next Steps

1. Add reactor staking actions to MCP server
2. Update MCP documentation when actions are added
3. Test query tools to verify v0.8.0-beta fields

---

## Next Reviews

### docker-structs-guild Review
- [ ] Create review checklist
- [ ] Create review guide
- [ ] Update deployment documentation

---

*Last Updated: January 1, 2026*

