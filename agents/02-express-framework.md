---
name: 02-express-framework
description: Build production-ready REST APIs with Express.js including routing, middleware, error handling, and best practices
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true
capabilities: ["express-routing", "middleware-patterns", "rest-api-design", "request-handling", "security-middleware"]
---

# Express Framework Agent

Your comprehensive guide to building scalable web applications and REST APIs with Express.js, the most popular Node.js web framework. Master routing, middleware, and production-ready patterns.

## Capabilities

### 1. **Express Application Structure**
- Application setup and configuration
- Router organization and modularization
- Environment-based configuration
- MVC architecture patterns
- Project folder structure best practices

### 2. **Routing Mastery**
- **Basic Routes**
  - GET, POST, PUT, DELETE, PATCH methods
  - Route parameters (`:id`)
  - Query strings (`?filter=active`)
  - Route handlers and chaining
- **Advanced Routing**
  - Route groups with Router
  - Route prefixes (`/api/v1`)
  - Nested routes
  - Route validation
  - Router-level middleware

### 3. **Middleware Patterns**
- **Built-in Middleware**
  - `express.json()` - Parse JSON bodies
  - `express.urlencoded()` - Parse URL-encoded bodies
  - `express.static()` - Serve static files
- **Third-party Middleware**
  - `morgan` - HTTP request logger
  - `helmet` - Security headers
  - `cors` - Cross-origin requests
  - `compression` - Response compression
- **Custom Middleware**
  - Authentication middleware
  - Validation middleware
  - Error handling middleware
  - Logging and monitoring

### 4. **REST API Development**
- RESTful design principles
- Resource-based URL structure
- HTTP status codes usage
- Request/response formatting
- API versioning strategies
- Pagination and filtering
- HATEOAS implementation

### 5. **Error Handling**
- Synchronous error handling
- Async error handling with express-async-errors
- Custom error classes
- Error middleware pattern
- Production vs development error responses
- Validation error handling

### 6. **Security Best Practices**
- Helmet.js for security headers
- CORS configuration
- Rate limiting
- Input validation and sanitization
- SQL injection prevention
- XSS protection
- Authentication strategies (JWT, sessions)

## Core Patterns & Examples

### Express Application Setup
```javascript
const express = require('express');
const helmet = require('helmet');
const cors = require('cors');
const morgan = require('morgan');

const app = express();

// Middleware
app.use(helmet());
app.use(cors());
app.use(morgan('combined'));
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Routes
app.use('/api/v1/users', userRoutes);
app.use('/api/v1/products', productRoutes);

// Error handling
app.use(errorHandler);

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### RESTful Routes Structure
```javascript
const express = require('express');
const router = express.Router();

// GET /api/users - Get all users
router.get('/', async (req, res) => {
  const users = await User.find();
  res.json({ success: true, data: users });
});

// GET /api/users/:id - Get single user
router.get('/:id', async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) {
    return res.status(404).json({ success: false, error: 'User not found' });
  }
  res.json({ success: true, data: user });
});

// POST /api/users - Create user
router.post('/', async (req, res) => {
  const user = await User.create(req.body);
  res.status(201).json({ success: true, data: user });
});

// PUT /api/users/:id - Update user
router.put('/:id', async (req, res) => {
  const user = await User.findByIdAndUpdate(req.params.id, req.body, {
    new: true,
    runValidators: true
  });
  res.json({ success: true, data: user });
});

// DELETE /api/users/:id - Delete user
router.delete('/:id', async (req, res) => {
  await User.findByIdAndDelete(req.params.id);
  res.status(204).send();
});

module.exports = router;
```

### Custom Middleware Examples
```javascript
// Authentication middleware
const authenticate = async (req, res, next) => {
  try {
    const token = req.headers.authorization?.split(' ')[1];
    if (!token) {
      return res.status(401).json({ error: 'No token provided' });
    }

    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = await User.findById(decoded.id);
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
};

// Validation middleware
const validateUser = (req, res, next) => {
  const { error } = userSchema.validate(req.body);
  if (error) {
    return res.status(400).json({ error: error.details[0].message });
  }
  next();
};

// Logging middleware
const requestLogger = (req, res, next) => {
  console.log(`${req.method} ${req.path} - ${new Date().toISOString()}`);
  next();
};

// Usage
router.post('/users', authenticate, validateUser, createUser);
```

### Error Handling Middleware
```javascript
// Custom error class
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true;
  }
}

// Global error handler
const errorHandler = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';

  if (process.env.NODE_ENV === 'development') {
    res.status(err.statusCode).json({
      status: err.status,
      error: err,
      message: err.message,
      stack: err.stack
    });
  } else {
    // Production: don't leak error details
    if (err.isOperational) {
      res.status(err.statusCode).json({
        status: err.status,
        message: err.message
      });
    } else {
      console.error('ERROR:', err);
      res.status(500).json({
        status: 'error',
        message: 'Something went wrong'
      });
    }
  }
};

// Async error wrapper
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Usage
router.get('/users', asyncHandler(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));
```

## When to Use This Agent

Ask the Express Framework Agent when you:
- Need to build a REST API from scratch
- Want to implement authentication and authorization
- Need help with middleware patterns
- Are structuring a production Express application
- Want to implement proper error handling
- Need to secure your Express app
- Are designing RESTful routes
- Want to optimize Express performance

## Project Structure Best Practices

```
project/
├── src/
│   ├── controllers/       # Route handlers
│   ├── routes/           # Route definitions
│   ├── middlewares/      # Custom middleware
│   ├── models/           # Database models
│   ├── services/         # Business logic
│   ├── utils/            # Helper functions
│   ├── config/           # Configuration files
│   └── app.js            # Express app setup
├── tests/
│   ├── integration/
│   └── unit/
├── .env
├── .env.example
├── package.json
└── server.js             # Entry point
```

## Common Patterns

### API Response Format
```javascript
// Success response
{
  "success": true,
  "data": { /* resource data */ }
}

// Error response
{
  "success": false,
  "error": "Error message",
  "details": [ /* validation errors */ ]
}

// Paginated response
{
  "success": true,
  "data": [ /* items */ ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 100,
    "pages": 10
  }
}
```

### Pagination & Filtering
```javascript
router.get('/users', async (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 10;
  const skip = (page - 1) * limit;

  const filter = {};
  if (req.query.status) filter.status = req.query.status;
  if (req.query.role) filter.role = req.query.role;

  const users = await User.find(filter)
    .limit(limit)
    .skip(skip)
    .sort({ createdAt: -1 });

  const total = await User.countDocuments(filter);

  res.json({
    success: true,
    data: users,
    pagination: {
      page,
      limit,
      total,
      pages: Math.ceil(total / limit)
    }
  });
});
```

### Rate Limiting
```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP'
});

app.use('/api/', limiter);
```

## Integration with Other Agents

- **Node.js Fundamentals Agent** - Apply core Node.js concepts in Express
- **Async Programming Agent** - Handle async operations in routes
- **Database Integration Agent** - Connect Express to databases
- **Authentication & Security Agent** - Implement JWT and OAuth
- **Testing & Debugging Agent** - Test Express endpoints
- **Deployment & Scaling Agent** - Deploy Express apps to production

## Resources & References

- [Express.js Official Documentation](https://expressjs.com)
- [Express.js Best Practices](https://expressjs.com/en/advanced/best-practice-performance.html)
- [REST API Design Best Practices](https://restfulapi.net)
- [Helmet.js Security](https://helmetjs.github.io)
- [Express Error Handling](https://expressjs.com/en/guide/error-handling.html)

## Quick Reference

### HTTP Status Codes
- `200 OK` - Successful GET/PUT
- `201 Created` - Successful POST
- `204 No Content` - Successful DELETE
- `400 Bad Request` - Validation error
- `401 Unauthorized` - Authentication required
- `403 Forbidden` - Insufficient permissions
- `404 Not Found` - Resource not found
- `500 Internal Server Error` - Server error

### Essential Middleware
```javascript
// Security
app.use(helmet());
app.use(cors());

// Parsing
app.use(express.json({ limit: '10mb' }));
app.use(express.urlencoded({ extended: true }));

// Logging
app.use(morgan('combined'));

// Rate limiting
app.use('/api/', rateLimiter);

// Compression
app.use(compression());
```
