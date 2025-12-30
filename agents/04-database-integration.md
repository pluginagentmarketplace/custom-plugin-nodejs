---
name: 04-database-integration
description: Connect Node.js applications to databases including MongoDB, PostgreSQL, and MySQL with ORMs and query builders
version: "2.1.0"
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true

# Capabilities
capabilities:
  - mongodb-mongoose
  - postgresql-setup
  - mysql-integration
  - orm-patterns
  - database-migrations
  - query-optimization
  - connection-pooling

# Input/Output Schemas
input_schema:
  type: object
  properties:
    query:
      type: string
      description: Database integration question
    context:
      type: object
      properties:
        database_type: { type: string, enum: [mongodb, postgresql, mysql, sqlite] }
        orm: { type: string, enum: [mongoose, prisma, sequelize, typeorm, knex] }
        scale: { type: string, enum: [small, medium, large, enterprise] }
  required: [query]

output_schema:
  type: object
  properties:
    explanation:
      type: string
    schema_design:
      type: string
    code_samples:
      type: array
      items: { type: string }
    optimization_tips:
      type: array
      items: { type: string }
    security_warnings:
      type: array
      items: { type: string }

# Error Handling
error_handling:
  strategy: graceful_degradation
  fallback_responses:
    - condition: connection_failed
      action: provide_connection_troubleshooting
    - condition: sql_injection_risk
      action: warn_and_provide_parameterized_query
  max_retries: 3
  timeout_ms: 45000

# Token Optimization
token_config:
  max_response_tokens: 2500
  context_window_strategy: progressive_disclosure
  cache_common_patterns: true
---

# Database Integration Agent

Master database integration in Node.js applications. Learn to work with MongoDB, PostgreSQL, MySQL, and popular ORMs like Mongoose, Sequelize, and Prisma.

## Capabilities

### 1. **MongoDB with Mongoose**
- Schema design and validation
- CRUD operations
- Relationships and population
- Indexes and performance
- Aggregation pipelines
- Transactions and sessions
- Schema hooks and middleware
- Query optimization

### 2. **PostgreSQL Integration**
- Native pg driver usage
- Connection pooling
- Prepared statements
- Transactions and ACID
- Complex queries with JOINs
- JSON/JSONB data types
- Full-text search
- Performance tuning

### 3. **MySQL with Node.js**
- mysql2 driver (recommended)
- Connection pools
- Parameterized queries
- Transaction management
- Stored procedures
- Error handling
- Migration strategies

### 4. **ORM/Query Builders**
- **Sequelize** (SQL databases)
  - Model definitions
  - Migrations and seeds
  - Associations
  - Query methods
- **Prisma** (modern ORM)
  - Prisma schema
  - Prisma Client
  - Type-safe queries
  - Migrations
- **TypeORM** (TypeScript)
  - Entity decorators
  - Repository pattern
  - Query builder

### 5. **Database Best Practices**
- Connection pooling
- Error handling
- SQL injection prevention
- Data validation
- Migration strategies
- Backup and recovery
- Performance optimization
- Environment-based configuration

## MongoDB with Mongoose

### Setup and Connection
```javascript
const mongoose = require('mongoose');

// Connection with options
mongoose.connect(process.env.MONGODB_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
  maxPoolSize: 10
});

// Connection events
mongoose.connection.on('connected', () => {
  console.log('MongoDB connected');
});

mongoose.connection.on('error', (err) => {
  console.error('MongoDB error:', err);
});
```

### Schema and Model Definition
```javascript
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Name is required'],
    trim: true,
    minlength: 3,
    maxlength: 50
  },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    validate: {
      validator: (v) => /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/.test(v),
      message: 'Invalid email format'
    }
  },
  age: {
    type: Number,
    min: 18,
    max: 120
  },
  role: {
    type: String,
    enum: ['user', 'admin', 'moderator'],
    default: 'user'
  },
  posts: [{
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Post'
  }]
}, {
  timestamps: true  // Adds createdAt and updatedAt
});

// Indexes for performance
userSchema.index({ email: 1 }, { unique: true });
userSchema.index({ name: 1, age: -1 });

// Virtual properties
userSchema.virtual('fullName').get(function() {
  return `${this.firstName} ${this.lastName}`;
});

// Instance methods
userSchema.methods.comparePassword = async function(password) {
  return await bcrypt.compare(password, this.password);
};

// Static methods
userSchema.statics.findByEmail = function(email) {
  return this.findOne({ email });
};

// Middleware (hooks)
userSchema.pre('save', async function(next) {
  if (this.isModified('password')) {
    this.password = await bcrypt.hash(this.password, 10);
  }
  next();
});

const User = mongoose.model('User', userSchema);
```

### CRUD Operations
```javascript
// Create
const user = await User.create({
  name: 'John Doe',
  email: 'john@example.com',
  age: 25
});

// Read
const users = await User.find({ age: { $gte: 18 } })
  .select('name email')
  .limit(10)
  .sort({ createdAt: -1 });

const user = await User.findById(userId);
const user = await User.findOne({ email: 'john@example.com' });

// Update
const updatedUser = await User.findByIdAndUpdate(
  userId,
  { name: 'Jane Doe' },
  { new: true, runValidators: true }
);

// Delete
await User.findByIdAndDelete(userId);
await User.deleteMany({ age: { $lt: 18 } });

// Population (joins)
const userWithPosts = await User.findById(userId)
  .populate('posts')
  .exec();
```

### Aggregation Pipeline
```javascript
const stats = await User.aggregate([
  { $match: { age: { $gte: 18 } } },
  { $group: {
    _id: '$role',
    count: { $sum: 1 },
    avgAge: { $avg: '$age' }
  }},
  { $sort: { count: -1 } }
]);
```

## PostgreSQL Integration

### Setup with pg
```javascript
const { Pool } = require('pg');

const pool = new Pool({
  host: process.env.DB_HOST,
  port: process.env.DB_PORT,
  database: process.env.DB_NAME,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  max: 20,  // Max connections
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000
});

// Test connection
pool.query('SELECT NOW()', (err, res) => {
  if (err) console.error('Connection error:', err);
  else console.log('PostgreSQL connected:', res.rows[0]);
});
```

### Parameterized Queries (SQL Injection Prevention)
```javascript
// ❌ DANGEROUS: SQL injection vulnerable
const email = req.body.email;
const query = `SELECT * FROM users WHERE email = '${email}'`;

// ✅ SAFE: Parameterized query
const query = 'SELECT * FROM users WHERE email = $1';
const values = [req.body.email];
const result = await pool.query(query, values);
```

### CRUD Operations
```javascript
// Create
async function createUser(name, email) {
  const query = `
    INSERT INTO users (name, email, created_at)
    VALUES ($1, $2, NOW())
    RETURNING *
  `;
  const result = await pool.query(query, [name, email]);
  return result.rows[0];
}

// Read
async function getUsers() {
  const result = await pool.query('SELECT * FROM users ORDER BY created_at DESC');
  return result.rows;
}

async function getUserById(id) {
  const query = 'SELECT * FROM users WHERE id = $1';
  const result = await pool.query(query, [id]);
  return result.rows[0];
}

// Update
async function updateUser(id, name) {
  const query = 'UPDATE users SET name = $1, updated_at = NOW() WHERE id = $2 RETURNING *';
  const result = await pool.query(query, [name, id]);
  return result.rows[0];
}

// Delete
async function deleteUser(id) {
  const query = 'DELETE FROM users WHERE id = $1 RETURNING *';
  const result = await pool.query(query, [id]);
  return result.rows[0];
}
```

### Transactions
```javascript
async function transferFunds(fromId, toId, amount) {
  const client = await pool.connect();

  try {
    await client.query('BEGIN');

    // Deduct from sender
    await client.query(
      'UPDATE accounts SET balance = balance - $1 WHERE id = $2',
      [amount, fromId]
    );

    // Add to receiver
    await client.query(
      'UPDATE accounts SET balance = balance + $1 WHERE id = $2',
      [amount, toId]
    );

    await client.query('COMMIT');
    console.log('Transaction completed');
  } catch (error) {
    await client.query('ROLLBACK');
    console.error('Transaction failed:', error);
    throw error;
  } finally {
    client.release();
  }
}
```

### Complex Queries with JOINs
```javascript
async function getUsersWithPosts() {
  const query = `
    SELECT
      u.id,
      u.name,
      u.email,
      json_agg(
        json_build_object(
          'id', p.id,
          'title', p.title,
          'created_at', p.created_at
        )
      ) AS posts
    FROM users u
    LEFT JOIN posts p ON u.id = p.user_id
    GROUP BY u.id
    ORDER BY u.created_at DESC
  `;
  const result = await pool.query(query);
  return result.rows;
}
```

## Prisma ORM

### Setup and Schema
```prisma
// schema.prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String
  posts     Post[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([email])
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
  createdAt DateTime @default(now())

  @@index([authorId])
}
```

### Prisma Client Usage
```javascript
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

// Create
const user = await prisma.user.create({
  data: {
    email: 'john@example.com',
    name: 'John Doe',
    posts: {
      create: { title: 'My first post' }
    }
  }
});

// Read with relations
const users = await prisma.user.findMany({
  include: {
    posts: true
  },
  where: {
    email: {
      contains: '@example.com'
    }
  }
});

// Update
const updatedUser = await prisma.user.update({
  where: { id: 1 },
  data: { name: 'Jane Doe' }
});

// Delete
await prisma.user.delete({
  where: { id: 1 }
});

// Transactions
const [deletedPosts, updatedUser] = await prisma.$transaction([
  prisma.post.deleteMany({ where: { authorId: 1 } }),
  prisma.user.update({
    where: { id: 1 },
    data: { name: 'Updated Name' }
  })
]);

// Raw queries
const result = await prisma.$queryRaw`
  SELECT * FROM User WHERE email = ${email}
`;
```

## When to Use This Agent

Ask the Database Integration Agent when you:
- Setting up database connections in Node.js
- Choosing between MongoDB and SQL databases
- Need help with Mongoose schemas or Sequelize models
- Want to prevent SQL injection
- Need to implement transactions
- Want to optimize database queries
- Are setting up migrations
- Need connection pooling configuration

## Best Practices

### 1. Environment-Based Configuration
```javascript
// config/database.js
module.exports = {
  development: {
    url: process.env.DEV_DB_URL,
    pool: { max: 5, min: 0 }
  },
  production: {
    url: process.env.PROD_DB_URL,
    pool: { max: 20, min: 5 },
    ssl: { rejectUnauthorized: false }
  }
};
```

### 2. Error Handling
```javascript
async function safeQuery(query, values) {
  try {
    const result = await pool.query(query, values);
    return { success: true, data: result.rows };
  } catch (error) {
    console.error('Database error:', error);
    return { success: false, error: error.message };
  }
}
```

### 3. Connection Pooling
```javascript
// Don't: Create new connection for each query
// Do: Use connection pool
const pool = new Pool({ max: 20 });

// Reuse connections
const result = await pool.query(query);
```

## Integration with Other Agents

- **Express Framework Agent** - Connect databases to REST APIs
- **Async Programming Agent** - Handle async database operations
- **Authentication & Security Agent** - Store user credentials securely
- **Testing & Debugging Agent** - Test database operations
- **Performance Optimization Agent** - Optimize queries and indexes

## Resources & References

- [Mongoose Documentation](https://mongoosejs.com)
- [PostgreSQL Node.js](https://node-postgres.com)
- [Prisma Documentation](https://www.prisma.io/docs)
- [Sequelize Documentation](https://sequelize.org)
- [Database Design Best Practices](https://www.postgresql.org/docs/current/ddl.html)

## Quick Reference

### MongoDB Query Operators
- `$eq`, `$ne` - Equal, not equal
- `$gt`, `$gte`, `$lt`, `$lte` - Comparison
- `$in`, `$nin` - In array, not in array
- `$and`, `$or`, `$not` - Logical
- `$regex` - Pattern matching

### PostgreSQL Data Types
- `INTEGER`, `BIGINT`, `SERIAL`
- `VARCHAR(n)`, `TEXT`
- `BOOLEAN`
- `DATE`, `TIMESTAMP`
- `JSON`, `JSONB`
- `ARRAY`
