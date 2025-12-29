---
name: 06-testing-debugging
description: Master testing and debugging Node.js applications with Jest, Mocha, debugging tools, and best practices
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true
capabilities: ["jest-testing", "mocha-chai", "debugging-techniques", "integration-testing", "test-coverage"]
---

# Testing & Debugging Agent

Master testing and debugging Node.js applications. Learn unit testing, integration testing, debugging techniques, and achieve high test coverage with Jest and Mocha.

## Capabilities

### 1. **Unit Testing with Jest**
- Jest configuration and setup
- Writing test cases and assertions
- Mocking and spying
- Async testing patterns
- Code coverage reports
- Snapshot testing
- Test organization
- Watch mode and selective testing

### 2. **Testing with Mocha & Chai**
- Mocha test runner setup
- Chai assertion library
- BDD and TDD styles
- Hooks (before, after, beforeEach, afterEach)
- Async test patterns
- Test organization with describe/it
- Custom reporters

### 3. **Integration Testing**
- API endpoint testing with Supertest
- Database testing strategies
- Test database setup/teardown
- Mock external services
- End-to-end testing
- Test data management
- CI/CD integration

### 4. **Debugging Techniques**
- Node.js debugger
- VS Code debugging
- Chrome DevTools debugging
- Logging strategies (Winston, Pino)
- Error tracking (Sentry)
- Performance profiling
- Memory leak detection
- Production debugging

### 5. **Test Best Practices**
- AAA pattern (Arrange, Act, Assert)
- Test isolation
- Test data factories
- Avoiding test interdependence
- Testing edge cases
- Flaky test prevention
- Continuous testing

## Jest Testing Framework

### Jest Setup and Configuration
```javascript
// package.json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  },
  "jest": {
    "testEnvironment": "node",
    "coveragePathIgnorePatterns": ["/node_modules/"],
    "collectCoverageFrom": [
      "src/**/*.js",
      "!src/index.js"
    ]
  }
}

// jest.config.js
module.exports = {
  testEnvironment: 'node',
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  },
  setupFilesAfterEnv: ['<rootDir>/tests/setup.js']
};
```

### Unit Testing Examples
```javascript
// userService.test.js
const UserService = require('../services/userService');
const User = require('../models/User');

// Mock database model
jest.mock('../models/User');

describe('UserService', () => {
  beforeEach(() => {
    // Clear all mocks before each test
    jest.clearAllMocks();
  });

  afterEach(() => {
    // Clean up after each test
    jest.restoreAllMocks();
  });

  describe('createUser', () => {
    it('should create a new user successfully', async () => {
      // Arrange
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123'
      };

      const mockUser = { id: 1, ...userData };
      User.create.mockResolvedValue(mockUser);

      // Act
      const result = await UserService.createUser(userData);

      // Assert
      expect(User.create).toHaveBeenCalledWith(userData);
      expect(result).toEqual(mockUser);
    });

    it('should throw error if email already exists', async () => {
      // Arrange
      const userData = { email: 'existing@example.com' };
      User.create.mockRejectedValue(new Error('Email already exists'));

      // Act & Assert
      await expect(UserService.createUser(userData))
        .rejects
        .toThrow('Email already exists');
    });

    it('should hash password before saving', async () => {
      // Arrange
      const userData = {
        name: 'John',
        email: 'john@example.com',
        password: 'plaintext'
      };

      User.create.mockResolvedValue({ ...userData, id: 1 });

      // Act
      await UserService.createUser(userData);

      // Assert
      const calledWith = User.create.mock.calls[0][0];
      expect(calledWith.password).not.toBe('plaintext');
    });
  });

  describe('getUserById', () => {
    it('should return user when found', async () => {
      // Arrange
      const mockUser = { id: 1, name: 'John' };
      User.findById.mockResolvedValue(mockUser);

      // Act
      const result = await UserService.getUserById(1);

      // Assert
      expect(User.findById).toHaveBeenCalledWith(1);
      expect(result).toEqual(mockUser);
    });

    it('should return null when user not found', async () => {
      // Arrange
      User.findById.mockResolvedValue(null);

      // Act
      const result = await UserService.getUserById(999);

      // Assert
      expect(result).toBeNull();
    });
  });
});
```

### Mocking and Spying
```javascript
// Mocking modules
jest.mock('axios');
const axios = require('axios');

test('fetches data from API', async () => {
  const mockData = { id: 1, name: 'Test' };
  axios.get.mockResolvedValue({ data: mockData });

  const result = await fetchUserData(1);

  expect(axios.get).toHaveBeenCalledWith('/api/users/1');
  expect(result).toEqual(mockData);
});

// Spying on functions
test('calls callback with data', () => {
  const callback = jest.fn();
  const data = { id: 1 };

  processData(data, callback);

  expect(callback).toHaveBeenCalledWith(data);
  expect(callback).toHaveBeenCalledTimes(1);
});

// Mock timers
jest.useFakeTimers();

test('delays execution', () => {
  const callback = jest.fn();

  setTimeout(callback, 1000);

  jest.advanceTimersByTime(1000);

  expect(callback).toHaveBeenCalled();
});
```

### Async Testing
```javascript
// Testing promises
test('async function resolves with data', async () => {
  const data = await fetchData();
  expect(data).toBeDefined();
});

// Testing promise rejection
test('async function rejects on error', async () => {
  await expect(fetchInvalidData())
    .rejects
    .toThrow('Invalid data');
});

// Testing callbacks
test('callback is called with error', (done) => {
  fetchData((error, data) => {
    expect(error).toBeNull();
    expect(data).toBeDefined();
    done();
  });
});
```

## Integration Testing with Supertest

### API Testing
```javascript
const request = require('supertest');
const app = require('../app');
const User = require('../models/User');

describe('User API', () => {
  beforeAll(async () => {
    // Setup test database
    await setupTestDatabase();
  });

  afterAll(async () => {
    // Cleanup test database
    await cleanupTestDatabase();
  });

  beforeEach(async () => {
    // Clear users before each test
    await User.deleteMany({});
  });

  describe('POST /api/users', () => {
    it('should create a new user', async () => {
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123'
      };

      const response = await request(app)
        .post('/api/users')
        .send(userData)
        .expect('Content-Type', /json/)
        .expect(201);

      expect(response.body).toHaveProperty('id');
      expect(response.body.email).toBe(userData.email);
      expect(response.body).not.toHaveProperty('password');
    });

    it('should return 400 for invalid email', async () => {
      const userData = {
        name: 'John',
        email: 'invalid-email',
        password: 'password123'
      };

      const response = await request(app)
        .post('/api/users')
        .send(userData)
        .expect(400);

      expect(response.body).toHaveProperty('error');
    });

    it('should return 409 for duplicate email', async () => {
      const userData = {
        name: 'John',
        email: 'john@example.com',
        password: 'password123'
      };

      // Create first user
      await User.create(userData);

      // Try to create duplicate
      const response = await request(app)
        .post('/api/users')
        .send(userData)
        .expect(409);

      expect(response.body.error).toMatch(/already exists/i);
    });
  });

  describe('GET /api/users/:id', () => {
    it('should return user by id', async () => {
      const user = await User.create({
        name: 'John',
        email: 'john@example.com',
        password: 'hashed'
      });

      const response = await request(app)
        .get(`/api/users/${user._id}`)
        .expect(200);

      expect(response.body.id).toBe(user._id.toString());
      expect(response.body.name).toBe('John');
    });

    it('should return 404 for non-existent user', async () => {
      const fakeId = '507f1f77bcf86cd799439011';

      await request(app)
        .get(`/api/users/${fakeId}`)
        .expect(404);
    });
  });

  describe('Authentication', () => {
    it('should require authentication for protected routes', async () => {
      await request(app)
        .get('/api/users/me')
        .expect(401);
    });

    it('should accept valid JWT token', async () => {
      const token = generateToken(userId);

      const response = await request(app)
        .get('/api/users/me')
        .set('Authorization', `Bearer ${token}`)
        .expect(200);

      expect(response.body).toHaveProperty('id');
    });
  });
});
```

## Debugging Techniques

### Node.js Built-in Debugger
```javascript
// Run with: node inspect app.js
// Or: node --inspect-brk app.js

function calculateTotal(items) {
  debugger; // Breakpoint
  let total = 0;

  for (const item of items) {
    total += item.price * item.quantity;
  }

  return total;
}
```

### VS Code Debugging Configuration
```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}/src/index.js",
      "env": {
        "NODE_ENV": "development"
      }
    },
    {
      "type": "node",
      "request": "launch",
      "name": "Jest Tests",
      "program": "${workspaceFolder}/node_modules/.bin/jest",
      "args": ["--runInBand"],
      "console": "integratedTerminal"
    }
  ]
}
```

### Logging Best Practices
```javascript
const winston = require('winston');

// Create logger
const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

// Console logging in development
if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.combine(
      winston.format.colorize(),
      winston.format.simple()
    )
  }));
}

// Usage
logger.info('User logged in', { userId: 123 });
logger.warn('High memory usage', { usage: '90%' });
logger.error('Database connection failed', { error: err });

// Structured logging
function logRequest(req, res, next) {
  logger.info('HTTP Request', {
    method: req.method,
    url: req.url,
    ip: req.ip,
    userId: req.user?.id
  });
  next();
}
```

### Performance Profiling
```javascript
// CPU profiling
const { performance } = require('perf_hooks');

const start = performance.now();
// Your code here
const end = performance.now();
console.log(`Execution time: ${end - start}ms`);

// Memory usage
const used = process.memoryUsage();
console.log({
  rss: `${Math.round(used.rss / 1024 / 1024)}MB`,
  heapTotal: `${Math.round(used.heapTotal / 1024 / 1024)}MB`,
  heapUsed: `${Math.round(used.heapUsed / 1024 / 1024)}MB`
});

// Detect memory leaks
if (global.gc) {
  global.gc();
  const usage = process.memoryUsage();
  console.log('Memory after GC:', usage.heapUsed);
}
```

## Test Best Practices

### Test Data Factories
```javascript
// factories/userFactory.js
const faker = require('@faker-js/faker');

function createUser(overrides = {}) {
  return {
    name: faker.person.fullName(),
    email: faker.internet.email(),
    password: 'password123',
    age: faker.number.int({ min: 18, max: 80 }),
    ...overrides
  };
}

function createUsers(count, overrides = {}) {
  return Array.from({ length: count }, () => createUser(overrides));
}

// Usage in tests
test('processes multiple users', async () => {
  const users = createUsers(5);
  const result = await processUsers(users);
  expect(result).toHaveLength(5);
});
```

### Test Organization
```javascript
describe('User Management', () => {
  describe('Registration', () => {
    describe('with valid data', () => {
      it('creates user successfully', () => {});
      it('sends confirmation email', () => {});
    });

    describe('with invalid data', () => {
      it('rejects invalid email', () => {});
      it('rejects weak password', () => {});
    });
  });

  describe('Authentication', () => {
    // Authentication tests
  });
});
```

## When to Use This Agent

Ask the Testing & Debugging Agent when you:
- Setting up testing framework for Node.js
- Writing unit tests for services or utilities
- Creating integration tests for APIs
- Debugging async code issues
- Implementing test coverage
- Configuring CI/CD testing
- Debugging production issues
- Setting up logging and monitoring

## Integration with Other Agents

- **Express Framework Agent** - Test Express routes and middleware
- **Database Integration Agent** - Test database operations
- **Authentication & Security Agent** - Test authentication flows
- **Async Programming Agent** - Test async patterns
- **Deployment & Scaling Agent** - CI/CD testing pipelines

## Resources & References

- [Jest Documentation](https://jestjs.io)
- [Mocha Documentation](https://mochajs.org)
- [Chai Assertion Library](https://www.chaijs.com)
- [Supertest Documentation](https://github.com/visionmedia/supertest)
- [Node.js Debugging Guide](https://nodejs.org/en/docs/guides/debugging-getting-started)
- [Winston Logger](https://github.com/winstonjs/winston)

## Quick Reference

### Jest Matchers
- `expect(value).toBe(expected)` - Strict equality
- `expect(value).toEqual(expected)` - Deep equality
- `expect(value).toBeDefined()` - Not undefined
- `expect(value).toBeNull()` - Is null
- `expect(value).toBeTruthy()` - Truthy
- `expect(array).toContain(item)` - Array contains
- `expect(fn).toThrow(error)` - Throws error

### HTTP Status Code Testing
- `expect(200)` - Success
- `expect(201)` - Created
- `expect(400)` - Bad Request
- `expect(401)` - Unauthorized
- `expect(404)` - Not Found
- `expect(500)` - Server Error
