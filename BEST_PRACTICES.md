# Best Practices Summary

**Version**: 1.0.0  
**Last Updated**: December 7, 2025  
**Purpose**: Quick reference of best practices for AI agents

---

## Overview

This document provides a concise summary of best practices for AI agents working with Structs. For detailed guidance, see the referenced documentation.

---

## Core Principles

### 1. Always Verify Actions

**Principle**: Transaction status `broadcast` does NOT mean action succeeded!

**Practice**:
- Always query game state after transactions
- Verify that the action actually occurred
- Check requirements if action failed

**See**: `protocols/action-protocol.md#validation-warning`

### 2. Load Documentation Efficiently

**Principle**: Load incrementally to maximize context window

**Practice**:
- Start with indexes and minimal schemas
- Load only what you need for current task
- Use entity-specific files when available

**See**: `LOADING_STRATEGY.md`

### 3. Handle Errors Gracefully

**Principle**: Categorize errors and handle appropriately

**Practice**:
- Distinguish retryable vs non-retryable errors
- Implement retry logic for transient errors
- Never retry non-retryable errors
- Log all errors with context

**See**: `protocols/error-handling.md`, `patterns/retry-strategies.md`

### 4. Respect Rate Limits

**Principle**: Monitor and stay within rate limits

**Practice**:
- Monitor rate limit headers
- Throttle requests to stay within limits
- Use caching to reduce API calls
- Handle 429 errors with appropriate delay

**See**: `patterns/rate-limiting.md`

### 5. Implement Caching

**Principle**: Cache responses to reduce API calls

**Practice**:
- Use appropriate TTL values by data type
- Invalidate cache on updates (if using streaming)
- Cache static data aggressively
- Don't cache real-time data

**See**: `patterns/caching.md`

### 6. Use Appropriate Data Access Method

**Principle**: Choose polling or streaming based on data characteristics

**Practice**:
- Use streaming for frequently-changing data
- Use polling for slowly-changing data
- Use hybrid approach (initial load + streaming)

**See**: `patterns/polling-vs-streaming.md`

### 7. Secure Credentials

**Principle**: Never expose credentials in code or logs

**Practice**:
- Use environment variables
- Store secrets securely
- Never log private keys
- Handle session expiration gracefully

**See**: `patterns/security.md`

### 8. Check Requirements Before Actions

**Principle**: Verify all requirements before submitting actions

**Practice**:
- Check player online status
- Verify resource availability
- Check power capacity
- Validate location/target
- Verify Command Ship status (if needed)
- Check fleet status (if needed)

**See**: `reference/action-quick-reference.md#common-requirements`

### 9. Use Patterns for Common Scenarios

**Principle**: Use established patterns instead of reinventing

**Practice**:
- Use pagination patterns for lists
- Use caching patterns for frequently accessed data
- Use retry patterns for failed requests
- Use workflow patterns for multi-step operations

**See**: `patterns/QUICK_REFERENCE.md`

### 10. Test Thoroughly

**Principle**: Test with real API before production

**Practice**:
- Test all endpoints you plan to use
- Test error scenarios
- Test edge cases
- Verify action results
- Monitor performance

**See**: `protocols/testing-protocol.md`

---

## Quick Reference

### Error Handling

**Retryable Errors**:
- `GENERAL_ERROR` (1)
- `INTERNAL_SERVER_ERROR` (500)
- `REQUEST_TIMEOUT`
- `NETWORK_ERROR`
- `RATE_LIMIT_EXCEEDED` (429) - Retry after delay

**Non-Retryable Errors**:
- `INSUFFICIENT_FUNDS` (2)
- `PLAYER_HALTED` (6)
- `INSUFFICIENT_CHARGE` (7)
- `INVALID_LOCATION` (8)
- `ENTITY_NOT_FOUND` (404)

**See**: `schemas/errors.json` for complete catalog

### Caching TTL Recommendations

- **Static data** (struct types): 1+ hour
- **Slowly-changing** (player info): 5 minutes
- **Moderately-changing** (guild info): 1 minute
- **Frequently-changing** (planet shield): 10-30 seconds

**See**: `patterns/caching.md` for detailed recommendations

### Rate Limits

- **Consensus Network**: 60 requests/minute
- **Web Application**: 100 requests/minute
- **RPC**: 30 requests/minute

**See**: `api/rate-limits.yaml` for complete details

### Common Requirements

**Player Requirements**:
- Player must be online (not halted)
- Sufficient resources available
- Sufficient power capacity

**Struct Requirements**:
- Sufficient charge (for actions)
- Sufficient power (for activation)
- Command Ship online (for planet building)
- Fleet on station (for planet building)

**See**: `reference/action-quick-reference.md` for complete requirements

---

## Common Pitfalls

### ❌ Don't Do This

- ❌ Assume transaction success (always verify)
- ❌ Ignore rate limits
- ❌ Retry non-retryable errors
- ❌ Hardcode credentials
- ❌ Skip requirement checking
- ❌ Cache real-time data
- ❌ Poll frequently-changing data
- ❌ Log sensitive data
- ❌ Ignore session expiration
- ❌ Skip error handling

### ✅ Do This Instead

- ✅ Always verify actions by querying game state
- ✅ Monitor and respect rate limits
- ✅ Only retry retryable errors
- ✅ Use environment variables for credentials
- ✅ Check all requirements before actions
- ✅ Use appropriate caching strategies
- ✅ Use streaming for real-time data
- ✅ Sanitize logs (never log private keys)
- ✅ Handle session expiration gracefully
- ✅ Implement comprehensive error handling

---

## Implementation Checklist

For a complete step-by-step implementation guide, see `IMPLEMENTATION_CHECKLIST.md`.

**Quick Checklist**:
- [ ] Security configured (credentials, sessions)
- [ ] Error handling implemented
- [ ] Retry logic implemented
- [ ] Caching implemented
- [ ] Rate limiting handled
- [ ] Requirements checking implemented
- [ ] Action verification implemented
- [ ] Testing completed
- [ ] Monitoring in place

---

## Related Documentation

**Comprehensive Guides**:
- `AGENTS.md` - Complete AI agent guide
- `IMPLEMENTATION_CHECKLIST.md` - Step-by-step checklist

**Patterns**:
- `patterns/QUICK_REFERENCE.md` - Pattern quick reference
- `patterns/README.md` - Pattern documentation index

**Protocols**:
- `protocols/error-handling.md` - Error handling
- `protocols/action-protocol.md` - Action patterns
- `protocols/query-protocol.md` - Query patterns

**Reference**:
- `reference/api-quick-reference.md` - API quick reference
- `reference/action-quick-reference.md` - Action quick reference

---

*Best Practices Summary Version: 1.0.0 - December 7, 2025*

