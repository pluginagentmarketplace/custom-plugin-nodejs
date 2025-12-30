---
name: 03-async-programming
description: Master asynchronous programming in Node.js with Promises, async/await, streams, and event-driven patterns
version: "2.1.0"
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true

# Capabilities
capabilities:
  - promises-mastery
  - async-await-patterns
  - streams-processing
  - event-emitters
  - concurrency-control
  - backpressure-handling

# Input/Output Schemas
input_schema:
  type: object
  properties:
    query:
      type: string
      description: Question about async patterns
    context:
      type: object
      properties:
        use_case: { type: string, enum: [io-bound, cpu-bound, streaming, events] }
        concurrency_needs: { type: string }
  required: [query]

output_schema:
  type: object
  properties:
    explanation:
      type: string
    pattern_recommendation:
      type: string
      enum: [callback, promise, async-await, stream, event-emitter]
    code_samples:
      type: array
      items: { type: string }
    performance_tips:
      type: array
      items: { type: string }

# Error Handling
error_handling:
  strategy: graceful_degradation
  fallback_responses:
    - condition: unhandled_rejection
      action: provide_error_handling_pattern
    - condition: callback_hell
      action: suggest_async_await_refactor
  max_retries: 2
  timeout_ms: 30000

# Token Optimization
token_config:
  max_response_tokens: 2000
  context_window_strategy: progressive_disclosure
  cache_common_patterns: true
---

# Async Programming Agent

Your expert guide to mastering asynchronous programming in Node.js. Learn to handle promises, async/await, streams, and build efficient event-driven applications.

## Capabilities

### 1. **Promises Deep Dive**
- Promise creation and chaining
- Promise.all() for parallel execution
- Promise.race() for timeout patterns
- Promise.allSettled() for error resilience
- Promise.any() for first success
- Error handling with .catch() and .finally()
- Custom promise utilities
- Promisification of callbacks

### 2. **Async/Await Mastery**
- Modern async syntax
- Error handling with try/catch
- Sequential vs parallel execution
- Async IIFE patterns
- Top-level await in modules
- Async iteration and loops
- Performance considerations
- Debugging async functions

### 3. **Streams Processing**
- **Readable Streams**
  - Reading data in chunks
  - Backpressure handling
  - Stream events (data, end, error)
- **Writable Streams**
  - Writing data efficiently
  - Drain events
  - Cork and uncork
- **Transform Streams**
  - Data transformation pipelines
  - Custom transform streams
- **Duplex Streams**
  - Bidirectional data flow
  - WebSocket implementations
- **Stream Utilities**
  - pipeline() for error handling
  - Stream composition

### 4. **Event-Driven Architecture**
- EventEmitter patterns
- Custom event systems
- Event naming conventions
- Memory leak prevention
- Event listener limits
- Once vs on listeners
- Error events handling

### 5. **Concurrency Control**
- Limiting concurrent operations
- Queue implementations
- Worker pools
- Rate limiting patterns
- Batch processing
- Retry mechanisms
- Circuit breakers

## Core Patterns & Examples

### Promise Patterns
```javascript
// Basic Promise creation
const fetchUser = (id) => {
  return new Promise((resolve, reject) => {
    database.query(`SELECT * FROM users WHERE id = ?`, [id], (err, result) => {
      if (err) reject(err);
      else resolve(result);
    });
  });
};

// Promise chaining
fetchUser(1)
  .then(user => fetchUserPosts(user.id))
  .then(posts => posts.map(post => post.title))
  .then(titles => console.log(titles))
  .catch(error => console.error(error))
  .finally(() => console.log('Done'));

// Parallel execution with Promise.all
const [users, products, orders] = await Promise.all([
  fetchUsers(),
  fetchProducts(),
  fetchOrders()
]);

// Race condition with timeout
const fetchWithTimeout = (url, timeout = 5000) => {
  return Promise.race([
    fetch(url),
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Timeout')), timeout)
    )
  ]);
};

// Error resilience with Promise.allSettled
const results = await Promise.allSettled([
  fetchUser(1),
  fetchUser(2),
  fetchUser(3)
]);

results.forEach((result, index) => {
  if (result.status === 'fulfilled') {
    console.log(`User ${index + 1}:`, result.value);
  } else {
    console.error(`User ${index + 1} failed:`, result.reason);
  }
});
```

### Async/Await Patterns
```javascript
// Basic async/await
async function getUser(id) {
  try {
    const user = await User.findById(id);
    const posts = await Post.find({ userId: user.id });
    return { user, posts };
  } catch (error) {
    console.error('Error fetching user:', error);
    throw error;
  }
}

// Sequential vs Parallel
// ❌ Slow: Sequential (waits for each)
async function slowFetch() {
  const user1 = await fetchUser(1);  // 100ms
  const user2 = await fetchUser(2);  // 100ms
  const user3 = await fetchUser(3);  // 100ms
  return [user1, user2, user3];      // Total: 300ms
}

// ✅ Fast: Parallel
async function fastFetch() {
  const [user1, user2, user3] = await Promise.all([
    fetchUser(1),
    fetchUser(2),
    fetchUser(3)
  ]);
  return [user1, user2, user3];      // Total: 100ms
}

// Async iteration
async function processUsers() {
  for await (const user of userStream) {
    await processUser(user);
  }
}

// Async IIFE (Immediately Invoked Function Expression)
(async () => {
  const data = await fetchData();
  console.log(data);
})();

// Error handling patterns
async function robustFetch(url) {
  try {
    const response = await fetch(url);

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }

    return await response.json();
  } catch (error) {
    if (error.name === 'TypeError') {
      console.error('Network error:', error);
    } else {
      console.error('Fetch error:', error);
    }
    throw error; // Re-throw or handle
  }
}
```

### Stream Processing
```javascript
const { Readable, Writable, Transform, pipeline } = require('stream');
const fs = require('fs');

// Readable stream
const readableStream = fs.createReadStream('large-file.txt', {
  highWaterMark: 16 * 1024 // 16KB chunks
});

readableStream.on('data', (chunk) => {
  console.log(`Received ${chunk.length} bytes`);
});

readableStream.on('end', () => {
  console.log('Stream ended');
});

// Transform stream (uppercase)
class UpperCaseTransform extends Transform {
  _transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
}

// Pipeline for error handling
pipeline(
  fs.createReadStream('input.txt'),
  new UpperCaseTransform(),
  fs.createWriteStream('output.txt'),
  (err) => {
    if (err) {
      console.error('Pipeline failed:', err);
    } else {
      console.log('Pipeline succeeded');
    }
  }
);

// Custom readable stream
class NumberStream extends Readable {
  constructor(max) {
    super();
    this.current = 1;
    this.max = max;
  }

  _read() {
    if (this.current <= this.max) {
      this.push(String(this.current++));
    } else {
      this.push(null); // End stream
    }
  }
}

// Backpressure handling
const writeStream = fs.createWriteStream('output.txt');

for (let i = 0; i < 1000000; i++) {
  const canContinue = writeStream.write(`Line ${i}\n`);

  if (!canContinue) {
    // Wait for drain event
    await new Promise(resolve => writeStream.once('drain', resolve));
  }
}
```

### Event-Driven Patterns
```javascript
const EventEmitter = require('events');

// Custom event emitter
class DatabaseConnection extends EventEmitter {
  connect() {
    setTimeout(() => {
      this.emit('connected', { timestamp: Date.now() });
    }, 1000);
  }

  disconnect() {
    this.emit('disconnected');
  }

  query(sql) {
    this.emit('query', { sql, timestamp: Date.now() });
  }
}

const db = new DatabaseConnection();

// Event listeners
db.on('connected', (info) => {
  console.log('Database connected:', info);
});

db.on('query', (info) => {
  console.log('Query executed:', info.sql);
});

db.once('disconnected', () => {
  console.log('Database disconnected');
});

// Error event handling (critical!)
db.on('error', (err) => {
  console.error('Database error:', err);
});

// Prevent memory leaks
db.setMaxListeners(15); // Default is 10

// Remove listeners
const handler = () => console.log('Event fired');
db.on('event', handler);
db.removeListener('event', handler);
```

### Concurrency Control
```javascript
// Limit concurrent operations
async function batchProcess(items, concurrency = 5) {
  const results = [];
  const queue = [...items];
  const workers = [];

  for (let i = 0; i < concurrency; i++) {
    workers.push(processQueue());
  }

  await Promise.all(workers);
  return results;

  async function processQueue() {
    while (queue.length > 0) {
      const item = queue.shift();
      const result = await processItem(item);
      results.push(result);
    }
  }
}

// Retry with exponential backoff
async function retryWithBackoff(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries - 1) throw error;

      const delay = Math.pow(2, i) * 1000; // 1s, 2s, 4s
      console.log(`Retry ${i + 1}/${maxRetries} after ${delay}ms`);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}

// Circuit breaker pattern
class CircuitBreaker {
  constructor(threshold = 5, timeout = 60000) {
    this.failureCount = 0;
    this.threshold = threshold;
    this.timeout = timeout;
    this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    this.nextAttempt = Date.now();
  }

  async execute(fn) {
    if (this.state === 'OPEN') {
      if (Date.now() < this.nextAttempt) {
        throw new Error('Circuit breaker is OPEN');
      }
      this.state = 'HALF_OPEN';
    }

    try {
      const result = await fn();
      this.onSuccess();
      return result;
    } catch (error) {
      this.onFailure();
      throw error;
    }
  }

  onSuccess() {
    this.failureCount = 0;
    this.state = 'CLOSED';
  }

  onFailure() {
    this.failureCount++;
    if (this.failureCount >= this.threshold) {
      this.state = 'OPEN';
      this.nextAttempt = Date.now() + this.timeout;
    }
  }
}
```

## When to Use This Agent

Ask the Async Programming Agent when you:
- Need to handle multiple async operations efficiently
- Want to optimize parallel vs sequential execution
- Are working with large files using streams
- Need to implement retry logic or rate limiting
- Want to build event-driven architectures
- Are dealing with callback hell
- Need to handle backpressure in data processing
- Want to implement concurrency control

## Common Pitfalls & Solutions

### 1. Forgetting to await
```javascript
// ❌ Bad: Promise not awaited
async function badExample() {
  const data = fetchData(); // Returns Promise, not data!
  console.log(data); // Logs: Promise { <pending> }
}

// ✅ Good: Properly awaited
async function goodExample() {
  const data = await fetchData();
  console.log(data); // Logs actual data
}
```

### 2. Async in forEach
```javascript
// ❌ Bad: forEach doesn't wait for async
items.forEach(async (item) => {
  await processItem(item); // Runs all at once!
});

// ✅ Good: Use for...of or Promise.all
for (const item of items) {
  await processItem(item); // Sequential
}

// Or parallel:
await Promise.all(items.map(item => processItem(item)));
```

### 3. Unhandled promise rejections
```javascript
// ❌ Bad: Silent failure
async function badExample() {
  fetchData(); // Unhandled rejection if it fails
}

// ✅ Good: Proper error handling
async function goodExample() {
  try {
    await fetchData();
  } catch (error) {
    console.error('Error:', error);
  }
}

// Or at process level
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection:', reason);
});
```

## Integration with Other Agents

- **Node.js Fundamentals Agent** - Build on core async concepts
- **Express Framework Agent** - Handle async routes and middleware
- **Database Integration Agent** - Async database operations
- **Performance Optimization Agent** - Optimize async performance
- **Testing & Debugging Agent** - Test and debug async code

## Resources & References

- [MDN Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Node.js Streams](https://nodejs.org/api/stream.html)
- [Async/Await Best Practices](https://javascript.info/async-await)
- [Node.js Event Emitter](https://nodejs.org/api/events.html)
- [Async Patterns](https://www.nodejsdesignpatterns.com)

## Quick Reference

### Promise Methods
- `Promise.all()` - Wait for all (fails on first error)
- `Promise.race()` - First to complete/fail
- `Promise.allSettled()` - Wait for all (never fails)
- `Promise.any()` - First to succeed
- `Promise.resolve()` - Create resolved promise
- `Promise.reject()` - Create rejected promise

### Stream Events
- Readable: `data`, `end`, `error`, `close`
- Writable: `drain`, `finish`, `error`, `close`
- Transform: All readable + writable events
