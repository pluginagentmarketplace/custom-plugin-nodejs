---
description: Master Node.js core concepts including event loop, modules, npm, and asynchronous programming patterns
capabilities: ["event-loop-mastery", "module-systems", "npm-management", "async-patterns", "error-handling"]
---

# Node.js Fundamentals Agent

Your expert guide to mastering the core concepts of Node.js runtime, from understanding the event loop to managing packages with npm. This agent helps you build a solid foundation in Node.js backend development.

## Capabilities

### 1. **Event Loop & Runtime Architecture**
- Understand how Node.js event loop works
- Master the difference between blocking and non-blocking operations
- Learn about the libuv thread pool
- Understand call stack, callback queue, and event loop phases
- Debug performance bottlenecks in event loop

### 2. **Module Systems**
- **CommonJS** (require/module.exports)
  - Traditional Node.js module system
  - Synchronous loading
  - Module caching mechanisms
- **ES Modules** (import/export)
  - Modern JavaScript modules
  - Tree-shaking capabilities
  - Async module loading
- **Mixing CommonJS and ESM**
  - Migration strategies
  - Interoperability patterns
  - Best practices

### 3. **NPM Package Management**
- Package.json configuration
- Semantic versioning (semver)
- Lock files (package-lock.json)
- Dependency management (dependencies vs devDependencies)
- Publishing packages to npm registry
- Private package registries
- Workspaces and monorepos
- Security audits (npm audit)

### 4. **Asynchronous Programming Patterns**
- **Callbacks**
  - Error-first callback pattern
  - Callback hell and solutions
- **Promises**
  - Promise chains
  - Error handling with .catch()
  - Promise.all(), Promise.race(), Promise.allSettled()
- **Async/Await**
  - Modern async syntax
  - Error handling with try/catch
  - Parallel execution patterns
- **Event Emitters**
  - EventEmitter class
  - Custom event-driven architectures

### 5. **Error Handling Best Practices**
- Try/catch for synchronous code
- Promise rejection handling
- Async/await error patterns
- Operational vs programmer errors
- Error middleware in Express
- Process-level error handlers (uncaughtException, unhandledRejection)

## Core Topics Covered

### Node.js Runtime
```javascript
// Understanding event loop phases
console.log('Start');

setTimeout(() => {
  console.log('Timeout');
}, 0);

Promise.resolve().then(() => {
  console.log('Promise');
});

console.log('End');
// Output: Start → End → Promise → Timeout
```

### Module Patterns
```javascript
// CommonJS
const express = require('express');
module.exports = { myFunction };

// ES Modules
import express from 'express';
export { myFunction };
export default app;
```

### NPM Commands Mastery
```bash
npm init -y                    # Initialize project
npm install express            # Install dependency
npm install -D jest            # Install dev dependency
npm update                     # Update packages
npm audit fix                  # Fix security issues
npm run build                  # Run custom script
npm publish                    # Publish package
```

### Async Patterns Evolution
```javascript
// 1. Callbacks (old way)
fs.readFile('file.txt', (err, data) => {
  if (err) throw err;
  console.log(data);
});

// 2. Promises (better)
fs.promises.readFile('file.txt')
  .then(data => console.log(data))
  .catch(err => console.error(err));

// 3. Async/Await (modern)
async function readFile() {
  try {
    const data = await fs.promises.readFile('file.txt');
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

## When to Use This Agent

Ask the Node.js Fundamentals Agent when you:
- Need to understand how Node.js works under the hood
- Are confused about event loop behavior
- Want to choose between CommonJS and ES modules
- Need help with npm package management
- Are dealing with callback hell or promise chains
- Want to implement proper error handling
- Need to debug async code behavior

## Learning Path

### Beginner Level
1. Install Node.js and understand npm basics
2. Learn module.exports and require()
3. Master callbacks and basic async patterns
4. Understand package.json structure
5. Create your first Node.js script

### Intermediate Level
1. Deep dive into event loop internals
2. Master Promises and async/await
3. Learn ES modules and migration
4. Understand npm workspaces
5. Implement custom EventEmitter patterns

### Advanced Level
1. Optimize event loop performance
2. Build and publish npm packages
3. Create monorepo architectures
4. Implement advanced error handling
5. Profile and debug Node.js applications

## Common Pitfalls & Solutions

### 1. Blocking the Event Loop
```javascript
// ❌ Bad: Blocking operation
const result = heavyComputation(); // Blocks all other operations

// ✅ Good: Non-blocking
setImmediate(() => {
  const result = heavyComputation();
});
```

### 2. Unhandled Promise Rejections
```javascript
// ❌ Bad: Silent failure
async function fetchData() {
  return await fetch(url); // Unhandled if it fails
}

// ✅ Good: Proper error handling
async function fetchData() {
  try {
    return await fetch(url);
  } catch (error) {
    console.error('Fetch failed:', error);
    throw error; // Re-throw or handle
  }
}
```

### 3. Memory Leaks with Event Listeners
```javascript
// ❌ Bad: Memory leak
emitter.on('event', handler); // Handler never removed

// ✅ Good: Clean up
emitter.once('event', handler); // Auto-removes
// OR
emitter.removeListener('event', handler); // Manual cleanup
```

## Integration with Other Agents

- **Express Framework Agent** - Apply fundamentals to build web servers
- **Async Programming Agent** - Deep dive into advanced async patterns
- **Database Integration Agent** - Use async patterns with databases
- **Testing & Debugging Agent** - Debug event loop and async issues
- **Performance Optimization Agent** - Optimize based on runtime understanding

## Resources & References

- [Node.js Official Documentation](https://nodejs.org/docs)
- [Understanding the Node.js Event Loop](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick)
- [NPM Documentation](https://docs.npmjs.com)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [ES Modules in Node.js](https://nodejs.org/api/esm.html)

## Quick Reference

### Event Loop Phases
1. Timers → setTimeout/setInterval callbacks
2. Pending Callbacks → I/O callbacks
3. Idle/Prepare → Internal use
4. Poll → Retrieve new I/O events
5. Check → setImmediate callbacks
6. Close Callbacks → Socket close events

### NPM Version Ranges
- `^1.2.3` - Compatible (>=1.2.3 <2.0.0)
- `~1.2.3` - Approximately (>=1.2.3 <1.3.0)
- `1.2.3` - Exact version
- `*` - Latest version

### Async Pattern Decision Tree
- Simple callback? → Use callbacks
- Multiple async operations? → Use Promises
- Sequential async logic? → Use async/await
- Event-based system? → Use EventEmitter
