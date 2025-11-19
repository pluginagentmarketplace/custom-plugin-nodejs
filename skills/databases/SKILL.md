---
name: databases-infrastructure
description: Master database design and management including SQL, NoSQL, data modeling, query optimization, and scaling. Learn when to use each database type.
---

# Databases & Infrastructure Skill

Master database systems, data modeling, and when to choose each technology.

## Quick Start

Database fundamentals:
1. **SQL vs NoSQL** - Relational vs document databases
2. **Data Modeling** - Designing efficient schemas
3. **Querying** - Writing efficient queries
4. **Indexing** - Performance optimization
5. **Scaling** - Handling growth

## Relational Databases (SQL)

### PostgreSQL (Most Recommended)
- **Type**: Open-source relational
- **Strengths**: Reliability, features, JSON support
- **Best For**: Web apps, analytics, complex queries
- **Adoption**: ⭐⭐⭐⭐⭐

Key features:
- ACID compliance
- JSON/JSONB data type
- Window functions
- Full-text search
- PostGIS for geospatial

### MySQL/MariaDB
- **Type**: Open-source relational
- **Strengths**: Speed, simplicity, widespread
- **Best For**: Web applications, WordPress
- **Adoption**: ⭐⭐⭐⭐⭐

Characteristics:
- Fast for read-heavy workloads
- Simple to set up
- Less features than PostgreSQL
- Wide hosting support

### SQL Server
- **Type**: Commercial relational (Microsoft)
- **Strengths**: Integration with Windows, performance
- **Best For**: Enterprise environments
- **Adoption**: ⭐⭐⭐⭐

Enterprise features:
- Integration with Active Directory
- Advanced analytics
- T-SQL specific features
- Windows ecosystem

## NoSQL Databases

### MongoDB
- **Type**: Document database (JSON-like)
- **Strengths**: Flexible schema, horizontal scaling
- **Best For**: Web apps, content management
- **Adoption**: ⭐⭐⭐⭐⭐

Document structure:
```json
{
  "_id": ObjectId("..."),
  "name": "John",
  "email": "john@example.com",
  "tags": ["user", "active"],
  "metadata": {
    "created": "2024-01-01",
    "updated": "2024-11-18"
  }
}
```

### Redis
- **Type**: In-memory key-value store
- **Strengths**: Speed, caching, real-time
- **Best For**: Caching, sessions, leaderboards
- **Adoption**: ⭐⭐⭐⭐⭐

Use cases:
- Session storage
- Cache layer
- Rate limiting
- Real-time leaderboards
- Message queue

### Cassandra
- **Type**: Distributed NoSQL
- **Strengths**: Horizontal scaling, availability
- **Best For**: Time-series data, massive scale
- **Adoption**: ⭐⭐⭐

Characteristics:
- Decentralized architecture
- High availability
- Eventually consistent
- Large datasets

### DynamoDB
- **Type**: AWS managed NoSQL
- **Strengths**: Serverless, managed, integrated with AWS
- **Best For**: AWS users, serverless applications
- **Adoption**: ⭐⭐⭐⭐

AWS-specific:
- Pay per request
- Auto-scaling
- Global tables
- Integration with other AWS services

## Data Modeling

### Entity-Relationship Diagram (ERD)
Basic structure:
```
User (1) ──── (Many) Posts
├── id (PK)        ├── id (PK)
├── name           ├── user_id (FK)
├── email          ├── title
└── created_at     ├── content
                   └── created_at

Post (1) ──── (Many) Comments
                ├── id (PK)
                ├── post_id (FK)
                ├── user_id (FK)
                ├── content
                └── created_at
```

### Normalization Levels

**1NF** - Eliminate repeating groups
✅ Each field contains single value
✅ No array columns

**2NF** - Remove partial dependencies
✅ All non-key attributes depend on full key
✅ Avoid redundant data

**3NF** - Remove transitive dependencies
✅ Non-key attributes depend only on key
✅ Eliminate data redundancy

**BCNF** - Stricter than 3NF
✅ Every determinant is candidate key
✅ For very normalized databases

### Denormalization
Sometimes intentional for performance:
- ✅ Reduces joins
- ✅ Improves read performance
- ❌ Increases storage
- ❌ Risks data inconsistency

## SQL Query Optimization

### Indexing Strategy

Types of indexes:
- **Primary Key** - Unique identifier
- **Foreign Key** - Relationship optimization
- **Unique Index** - Constraint + performance
- **Full-Text Index** - Text searching
- **Composite Index** - Multi-column queries

```sql
-- Examples
CREATE INDEX idx_user_email ON users(email);
CREATE UNIQUE INDEX idx_email ON users(email);
CREATE INDEX idx_post_user_date
  ON posts(user_id, created_at DESC);
CREATE FULLTEXT INDEX idx_content
  ON articles(title, body);
```

### Query Optimization

```sql
-- Bad: Full table scan
SELECT * FROM users WHERE email LIKE '%@gmail.com';

-- Better: Use indexed column properly
SELECT * FROM users WHERE email = 'john@gmail.com';

-- Bad: Multiple joins without consideration
SELECT * FROM users u
JOIN posts p ON u.id = p.user_id
JOIN comments c ON p.id = c.post_id
WHERE u.id = 1;

-- Better: Filter early
SELECT * FROM posts p
WHERE p.user_id = 1
  AND p.id IN (
    SELECT post_id FROM comments
  );

-- Bad: SELECT * with many columns
SELECT * FROM large_table;

-- Better: Select specific columns
SELECT id, name, email FROM users;
```

### EXPLAIN and Query Analysis
```sql
-- Analyze query performance
EXPLAIN ANALYZE
SELECT u.name, COUNT(p.id) as post_count
FROM users u
LEFT JOIN posts p ON u.id = p.user_id
GROUP BY u.id
ORDER BY post_count DESC;
```

## Database Comparison

| DB Type | Use Case | Scalability | Consistency | Complexity |
|---------|----------|-------------|-------------|-----------|
| PostgreSQL | General web apps | Vertical | Strong ACID | Medium |
| MySQL | Web apps, CMS | Vertical | ACID (InnoDB) | Low |
| MongoDB | Flexible schema | Horizontal | Eventual | Low-Medium |
| Redis | Caching, sessions | Vertical | Strong | Low |
| Cassandra | Time-series, big data | Horizontal | Eventual | High |
| DynamoDB | AWS serverless | Horizontal | Eventual | Medium |

## Scaling Strategies

### Vertical Scaling (Bigger Machine)
```
Add more CPU, RAM to single server
✅ Simple
❌ Hardware limits
❌ Single point of failure
```

### Horizontal Scaling (More Machines)
```
Add more servers (replication/sharding)
✅ Unlimited capacity
✅ Redundancy
❌ Complex
❌ Data consistency challenges
```

### Replication
```
Primary Server → Replica 1
             → Replica 2
             → Replica 3

✅ Read scaling
✅ Redundancy
❌ Replication lag
❌ Consistency challenges
```

### Sharding
```
Data partitioned by key range:
Shard 1: Users A-M
Shard 2: Users N-Z

✅ Horizontal scaling
❌ Complex operations
❌ Data redistribution (rebalancing)
❌ Cross-shard queries difficult
```

## Database Selection Guide

### Choose SQL if...
- Need strict consistency
- Complex queries/relationships
- Data structure is known
- ACID compliance required

### Choose NoSQL if...
- Schema is flexible/evolving
- Need horizontal scaling
- High volume of simple reads/writes
- Can accept eventual consistency

### Choose Redis if...
- Need fast caching
- Working with sessions
- Real-time features
- Temporary data storage

### Choose Cassandra if...
- Time-series data
- Massive scale required
- Availability critical
- Can accept eventual consistency

## Common Mistakes

- ❌ Choosing NoSQL without understanding trade-offs
- ❌ Skipping indexes for "future optimization"
- ❌ Writing unoptimized queries at scale
- ❌ Denormalizing prematurely
- ❌ Not backing up data
- ❌ Ignoring replication lag
- ❌ Putting everything in one database

## Best Practices

✅ Index frequently queried columns
✅ Write optimized queries from start
✅ Monitor slow query logs
✅ Plan for scaling early
✅ Regular backups
✅ Use connection pooling
✅ Profile before optimizing
✅ Document schema changes
✅ Use migrations for schema updates

## Learning Resources

**SQL**
- Mode SQL Tutorial
- LeetCode SQL problems
- PostgreSQL Official Docs

**NoSQL**
- MongoDB University
- Redis Official Docs
- DynamoDB Developer Guide

**Tools**
- DBeaver (Universal SQL client)
- MongoDB Compass
- Redis Insight
