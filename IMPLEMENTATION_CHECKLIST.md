# Implementation Checklist

**Version**: 1.0.0  
**Last Updated**: December 7, 2025  
**Purpose**: Step-by-step checklist for AI agents implementing Structs bots

---

## Overview

This checklist provides a structured approach to implementing a Structs bot. Follow these steps in order to ensure you have all the necessary components in place.

**Use this checklist** alongside `AGENTS.md` for comprehensive guidance.

---

## Phase 1: Setup and Configuration

### 1.1 Documentation Loading

- [ ] Read `LOADING_STRATEGY.md` - Understand efficient loading
- [ ] Read `AGENTS.md` - Comprehensive agent guide
- [ ] Review `DOCUMENTATION_INDEX.md` - Discover available docs
- [ ] Load minimal schemas: `schemas/minimal/` for essential info

### 1.2 Environment Setup

- [ ] Set up development environment
- [ ] Configure API base URLs:
  - [ ] Consensus Network: `http://localhost:1317`
  - [ ] Web Application: `http://localhost:8080`
  - [ ] Streaming: `nats://localhost:4222` or `ws://localhost:1443`
- [ ] Configure network settings (if needed)

### 1.3 Security Configuration

- [ ] Set up environment variables for credentials
- [ ] Store private keys securely (never in code)
- [ ] Configure session storage (if using webapp API)
- [ ] Review `patterns/security.md` for security best practices

**See**: `patterns/security.md` for detailed security guidance

---

## Phase 2: Core Functionality

### 2.1 Query Implementation

- [ ] Read `protocols/query-protocol.md` - Query patterns
- [ ] Review `reference/api-quick-reference.md` - Common endpoints
- [ ] Implement player query: `GET /structs/player/{id}`
- [ ] Implement planet query: `GET /structs/planet/{id}`
- [ ] Implement struct query: `GET /structs/struct/{id}`
- [ ] Test queries with real API

**See**: `api/queries/` for entity-specific query endpoints

### 2.2 Error Handling

- [ ] Read `protocols/error-handling.md` - Error handling patterns
- [ ] Review `schemas/errors.json` - Error code definitions
- [ ] Implement error detection (check response status)
- [ ] Categorize errors (retryable vs non-retryable)
- [ ] Implement basic error logging

**See**: `examples/errors/` for error examples

### 2.3 Retry Logic

- [ ] Read `patterns/retry-strategies.md` - Retry patterns
- [ ] Implement exponential backoff for retryable errors
- [ ] Set max retry limits (recommended: 3)
- [ ] Handle rate limit errors (429) with fixed delay
- [ ] Test retry logic with various error scenarios

**See**: `patterns/retry-strategies.md` for retry implementation

---

## Phase 3: Performance Optimization

### 3.1 Caching

- [ ] Read `patterns/caching.md` - Caching strategies
- [ ] Implement response caching
- [ ] Set appropriate TTL values:
  - [ ] Static data: 1+ hour
  - [ ] Slowly-changing: 5 minutes
  - [ ] Frequently-changing: 10-30 seconds
- [ ] Implement cache invalidation (if using streaming)
- [ ] Test cache hit/miss rates

**See**: `patterns/caching.md` for caching implementation

### 3.2 Rate Limiting

- [ ] Read `patterns/rate-limiting.md` - Rate limit handling
- [ ] Implement rate limit monitoring (check headers)
- [ ] Implement request throttling
- [ ] Handle 429 responses appropriately
- [ ] Monitor rate limit usage

**See**: `api/rate-limits.yaml` for rate limit details

### 3.3 Data Access Strategy

- [ ] Read `patterns/polling-vs-streaming.md` - Decision guide
- [ ] Choose polling or streaming for each data type
- [ ] Implement chosen strategy
- [ ] Consider hybrid approach (initial load + streaming)
- [ ] Test data freshness and latency

**See**: `patterns/polling-vs-streaming.md` for decision framework

---

## Phase 4: Action Implementation

### 4.1 Transaction Flow

- [ ] Read `protocols/action-protocol.md` - Action patterns
- [ ] Review `reference/action-quick-reference.md` - Action reference
- [ ] Implement account info query
- [ ] Implement transaction building
- [ ] Implement transaction signing
- [ ] Implement transaction submission
- [ ] Implement confirmation waiting
- [ ] Implement action verification (query game state)

**See**: `schemas/actions.json` for action definitions

### 4.2 Action Requirements

- [ ] Review action requirements in `schemas/actions.json`
- [ ] Implement requirement checking before actions:
  - [ ] Player online check
  - [ ] Resource availability check
  - [ ] Power capacity check
  - [ ] Location validity check
  - [ ] Command Ship status (if needed)
  - [ ] Fleet status (if needed)
- [ ] Handle requirement failures gracefully

**See**: `reference/action-quick-reference.md` for common requirements

### 4.3 Proof-of-Work

- [ ] Understand proof-of-work requirements
- [ ] Implement proof-of-work computation (if needed)
- [ ] Test proof-of-work for mining/refining actions
- [ ] Test proof-of-work for build completion

**See**: `protocols/action-protocol.md#proof-of-work` for details

---

## Phase 5: Workflow Implementation

### 5.1 Workflow Patterns

- [ ] Read `patterns/workflow-patterns.md` - Workflow patterns
- [ ] Review `examples/workflows/README.md` - Workflow examples
- [ ] Identify workflow dependencies
- [ ] Implement sequential workflows (if needed)
- [ ] Implement parallel workflows (if applicable)
- [ ] Implement workflow state management

**See**: `examples/workflows/` for complete workflow examples

### 5.2 Common Workflows

- [ ] Implement "Get Player and Planets" workflow
- [ ] Implement "Build Struct" workflow (if needed)
- [ ] Implement "Monitor Planet Shield" workflow (if needed)
- [ ] Implement "Authenticated Guild Query" workflow (if needed)
- [ ] Test each workflow end-to-end

**See**: `examples/workflows/` for workflow examples

---

## Phase 6: Authentication

### 6.1 Webapp Authentication

- [ ] Read `protocols/authentication.md` - Authentication patterns
- [ ] Review `examples/auth/webapp-login.json` - Login example
- [ ] Implement login flow
- [ ] Implement session storage
- [ ] Implement session expiration handling
- [ ] Implement authenticated requests

**See**: `examples/auth/` for authentication examples

### 6.2 Transaction Signing

- [ ] Review `examples/auth/consensus-transaction-signing.json`
- [ ] Implement transaction signing
- [ ] Test signing with real transactions
- [ ] Handle signing errors

### 6.3 Streaming Authentication

- [ ] Review `examples/auth/nats-connection.json`
- [ ] Implement NATS connection (if using streaming)
- [ ] Handle connection failures
- [ ] Implement reconnection logic

---

## Phase 7: Streaming (If Applicable)

### 7.1 Streaming Setup

- [ ] Read `protocols/streaming.md` - Streaming protocol
- [ ] Review `api/streaming/` - Streaming documentation
- [ ] Implement NATS connection
- [ ] Test connection stability
- [ ] Implement reconnection logic

### 7.2 Event Subscription

- [ ] Review `api/streaming/event-types.yaml` - Event types
- [ ] Review `api/streaming/subscription-patterns.yaml` - Patterns
- [ ] Implement event subscriptions
- [ ] Implement event processing
- [ ] Test event handling

### 7.3 Event Integration

- [ ] Integrate streaming with caching (invalidate on events)
- [ ] Integrate streaming with workflows
- [ ] Test real-time updates
- [ ] Monitor event processing performance

---

## Phase 8: Testing and Validation

### 8.1 Unit Testing

- [ ] Test query functions
- [ ] Test action functions
- [ ] Test error handling
- [ ] Test retry logic
- [ ] Test caching

### 8.2 Integration Testing

- [ ] Test complete workflows
- [ ] Test authentication flows
- [ ] Test streaming (if applicable)
- [ ] Test error recovery
- [ ] Test rate limit handling

### 8.3 Validation Testing

- [ ] Verify actions actually occurred (query game state)
- [ ] Test requirement checking
- [ ] Test proof-of-work (if applicable)
- [ ] Test edge cases
- [ ] Test error scenarios

**See**: `protocols/testing-protocol.md` for testing patterns

---

## Phase 9: Monitoring and Optimization

### 9.1 Performance Monitoring

- [ ] Monitor API call frequency
- [ ] Monitor rate limit usage
- [ ] Monitor cache hit rates
- [ ] Monitor response times
- [ ] Monitor error rates

### 9.2 Optimization

- [ ] Optimize caching TTL values
- [ ] Optimize request frequency
- [ ] Optimize workflow execution
- [ ] Optimize error handling
- [ ] Optimize documentation loading

### 9.3 Logging

- [ ] Implement comprehensive logging
- [ ] Log all API requests/responses
- [ ] Log errors with context
- [ ] Log workflow progress
- [ ] Ensure sensitive data is not logged

**See**: `patterns/security.md` for secure logging practices

---

## Phase 10: Documentation and Maintenance

### 10.1 Code Documentation

- [ ] Document code structure
- [ ] Document configuration
- [ ] Document workflows
- [ ] Document error handling
- [ ] Document performance optimizations

### 10.2 Maintenance

- [ ] Set up monitoring alerts
- [ ] Plan for credential rotation
- [ ] Plan for API changes
- [ ] Plan for error recovery
- [ ] Plan for scaling

---

## Quick Reference

### Essential Reading Order

1. `LOADING_STRATEGY.md` - Efficient loading
2. `AGENTS.md` - Comprehensive guide
3. `protocols/query-protocol.md` - Query patterns
4. `protocols/action-protocol.md` - Action patterns
5. `protocols/error-handling.md` - Error handling
6. `patterns/retry-strategies.md` - Retry logic
7. `patterns/caching.md` - Caching
8. `patterns/rate-limiting.md` - Rate limits
9. `patterns/workflow-patterns.md` - Workflows
10. `patterns/security.md` - Security

### Essential Files to Load

**Minimal Load** (~200 lines):
- `reference/endpoint-index.json`
- `reference/action-index.json`
- `schemas/formats.json`

**Essential Load** (~500 lines):
- Minimal load files
- `schemas/minimal/player-essential.json`
- `schemas/minimal/planet-essential.json`
- `schemas/minimal/struct-essential.json`
- Essential sections of protocols

**Task-Specific Load** (~600-1000 lines):
- Load only what's needed for current task
- Use entity-specific schemas: `schemas/entities/{entity}.json`
- Use entity-specific queries: `api/queries/{entity}.yaml`

**See**: `LOADING_STRATEGY.md` for detailed loading strategies

---

## Common Pitfalls to Avoid

### ❌ Don't Do This

- ❌ Hardcode credentials in code
- ❌ Ignore rate limits
- ❌ Skip error handling
- ❌ Assume transaction success (always verify)
- ❌ Cache everything (use appropriate TTL)
- ❌ Poll frequently-changing data (use streaming)
- ❌ Retry non-retryable errors
- ❌ Skip requirement checking
- ❌ Ignore session expiration
- ❌ Log sensitive data

### ✅ Do This Instead

- ✅ Use environment variables for credentials
- ✅ Monitor and respect rate limits
- ✅ Implement comprehensive error handling
- ✅ Always verify actions by querying game state
- ✅ Use appropriate caching strategies
- ✅ Choose streaming for real-time data
- ✅ Only retry retryable errors
- ✅ Check all requirements before actions
- ✅ Handle session expiration gracefully
- ✅ Sanitize logs (never log private keys)

---

## Success Criteria

Your implementation is ready when:

- ✅ All queries work correctly
- ✅ All actions work correctly
- ✅ Error handling is comprehensive
- ✅ Retry logic works for transient errors
- ✅ Caching reduces API calls
- ✅ Rate limits are respected
- ✅ Actions are verified after submission
- ✅ Security best practices are followed
- ✅ Workflows execute successfully
- ✅ Performance is optimized
- ✅ Monitoring is in place
- ✅ Documentation is complete

---

## Related Documentation

**Getting Started**:
- `AGENTS.md` - Comprehensive agent guide
- `LOADING_STRATEGY.md` - Efficient loading
- `DOCUMENTATION_INDEX.md` - Complete index

**Patterns**:
- `patterns/QUICK_REFERENCE.md` - Pattern quick reference
- `patterns/README.md` - Pattern documentation index

**Examples**:
- `examples/workflows/README.md` - Workflow examples
- `examples/errors/` - Error examples
- `examples/auth/` - Authentication examples

**Reference**:
- `reference/api-quick-reference.md` - API quick reference
- `reference/action-quick-reference.md` - Action quick reference

---

*Implementation Checklist Version: 1.0.0 - December 7, 2025*

