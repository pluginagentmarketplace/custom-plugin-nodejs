---
name: error-handling
description: Node.js error handling patterns and best practices
sasmp_version: "1.3.0"
bonded_agent: 06-testing-debugging
bond_type: PRIMARY_BOND
---

# Node.js Error Handling Skill

## Overview
Implement robust error handling in Node.js applications using proper patterns for sync and async code.

## Topics Covered

### Error Types
- Operational vs programmer errors
- System errors (errno)
- Custom error classes
- Error inheritance
- Error serialization

### Sync Error Handling
- Try-catch patterns
- Error propagation
- Synchronous callbacks
- Error first callbacks
- Throw vs return errors

### Async Error Handling
- Promise rejection handling
- Async/await try-catch
- Unhandled rejections
- Event error handling
- Stream error handling

### Express Error Handling
- Error middleware
- Async error wrapper
- Centralized error handling
- Error response formatting
- HTTP status codes

### Production Practices
- Error logging strategies
- Error monitoring (Sentry)
- Graceful shutdown
- Process managers
- Health checks

## Prerequisites
- Node.js fundamentals
- Async programming

## Learning Outcomes
- Handle errors gracefully
- Create custom error classes
- Implement error middleware
- Set up error monitoring
