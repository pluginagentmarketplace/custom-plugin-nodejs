# Node.js Developer Pro - Claude Code Plugin

Complete Node.js backend development mastery from fundamentals to production deployment. Master Express REST APIs, databases, authentication, testing, Docker, and performance optimization based on the official [Node.js Developer Roadmap](https://roadmap.sh/nodejs).

## Overview

**Node.js Developer Pro** is a comprehensive Claude Code plugin designed to guide you through modern backend development with Node.js:

- **7 Specialized Agents** - Expert guidance across all aspects of Node.js development
- **7 Core Skills** - Hands-on skills covering Express, MongoDB, JWT, Testing, Docker, and more
- **4 Interactive Commands** - Quick-start guides for common Node.js tasks
- **Production-Ready Patterns** - Real-world examples and best practices
- **100% Backend Focus** - No frontend or Python content

## Key Features

### 7 Expert Agents

1. **Node.js Fundamentals** - Event loop, modules, npm, and async programming
2. **Express Framework** - REST APIs, routing, middleware, error handling
3. **Async Programming** - Promises, async/await, streams, event-driven patterns
4. **Database Integration** - MongoDB, PostgreSQL, MySQL, ORMs (Mongoose, Sequelize, Prisma)
5. **Authentication & Security** - JWT, OAuth, sessions, security best practices
6. **Testing & Debugging** - Jest, Mocha, Supertest, debugging techniques
7. **Deployment & Scaling** - Docker, PM2, clustering, load balancing, cloud platforms

### 7 Core Skills

1. **Express REST API** - Build production-ready RESTful APIs
2. **Async Patterns** - Master asynchronous JavaScript
3. **Mongoose & MongoDB** - NoSQL database integration
4. **JWT Authentication** - Secure authentication flows
5. **Jest Testing** - Comprehensive test coverage
6. **Docker Deployment** - Containerize Node.js applications
7. **Performance Optimization** - Caching, clustering, profiling

### 4 Quick-Start Commands

```bash
/nodejs-start      # Get started with Node.js development
/nodejs-api        # Build a REST API with Express + MongoDB
/nodejs-test       # Setup testing with Jest
/nodejs-deploy     # Deploy to production with Docker
```

## Quick Start

### Installation

```bash
# Clone or install the plugin
git clone https://github.com/pluginagentmarketplace/custom-plugin-nodejs.git

# In Claude Code, add the plugin
plugin add ./custom-plugin-nodejs
```

### First Steps

1. **Learn Fundamentals**
   - Understand Node.js event loop
   - Master async/await patterns
   - Set up your first Express server

2. **Build an API**
   - Create REST endpoints
   - Connect to MongoDB
   - Implement JWT authentication

3. **Test Your Code**
   - Write unit tests with Jest
   - Test APIs with Supertest
   - Achieve 80%+ coverage

4. **Deploy to Production**
   - Containerize with Docker
   - Use PM2 for process management
   - Deploy to cloud platforms

## What You'll Learn

### Backend Foundations
- Node.js runtime architecture
- Event loop and non-blocking I/O
- Module systems (CommonJS and ES Modules)
- NPM package management
- Environment configuration

### REST API Development
- Express.js framework
- RESTful design principles
- Route organization (MVC pattern)
- Middleware patterns
- Request validation
- Error handling
- API versioning

### Database Integration
- **MongoDB** with Mongoose ODM
  - Schema design
  - CRUD operations
  - Relationships and population
  - Aggregation pipelines
- **PostgreSQL** with pg/Sequelize
  - Parameterized queries
  - Transactions
  - JOINs and complex queries
- **Prisma** modern ORM
  - Type-safe database access
  - Auto-generated migrations

### Authentication & Security
- JWT token-based authentication
- Access and refresh tokens
- Password hashing with bcrypt
- OAuth 2.0 integration
- Role-based access control (RBAC)
- Security headers (Helmet.js)
- CORS configuration
- Rate limiting
- Input validation

### Testing Strategies
- Unit testing with Jest
- Integration testing with Supertest
- Mocking and spying
- Test coverage reports
- CI/CD integration
- TDD/BDD approaches

### Deployment & DevOps
- Docker containerization
- Multi-stage Docker builds
- Docker Compose for services
- PM2 process management
- Clustering for multi-core usage
- Nginx reverse proxy
- Load balancing strategies
- Cloud deployment (AWS, Heroku, DigitalOcean)
- CI/CD pipelines

### Performance Optimization
- Caching with Redis
- Database query optimization
- Response compression
- Connection pooling
- CPU profiling
- Memory leak detection
- Monitoring and logging

## Learning Paths

### Beginner Path (3-4 months)

**Month 1: Fundamentals**
- Install Node.js and npm
- Understand event loop
- Learn async/await
- Build first Express server
- Connect to MongoDB

**Month 2: REST APIs**
- Design RESTful endpoints
- Implement CRUD operations
- Add input validation
- Error handling
- Pagination and filtering

**Month 3: Authentication & Testing**
- Implement JWT authentication
- User registration/login
- Protected routes
- Write Jest tests
- API testing with Supertest

**Month 4: Deployment**
- Dockerize application
- Use PM2 for production
- Deploy to cloud
- Setup monitoring

### Intermediate Path (5-6 months)

**Months 1-3: Build Complete Projects**
- RESTful blog API
- E-commerce backend
- Social media API
- Real-time chat server

**Months 4-5: Advanced Topics**
- Microservices architecture
- Event-driven systems
- GraphQL integration
- WebSocket servers
- Background job processing

**Month 6: Production Mastery**
- Load balancing
- Auto-scaling
- Performance tuning
- Security hardening
- Monitoring and alerting

### Advanced Path (6-12 months)

- Microservices with Docker Swarm/Kubernetes
- Message queues (RabbitMQ, Kafka)
- Serverless architecture
- Performance optimization at scale
- System design for Node.js
- Contributing to open source

## Real-World Projects

### Project 1: Task Management API
**Tech Stack**: Express + MongoDB + JWT

- User authentication
- CRUD operations for tasks
- Task assignment and status
- Filtering and pagination
- Role-based permissions

### Project 2: E-Commerce Backend
**Tech Stack**: Express + PostgreSQL + Stripe

- Product catalog
- Shopping cart
- Payment processing
- Order management
- Inventory tracking
- Admin dashboard

### Project 3: Social Media API
**Tech Stack**: Express + MongoDB + Socket.io

- User profiles
- Post creation and feeds
- Comments and likes
- Real-time notifications
- File uploads (S3)
- Search functionality

### Project 4: Analytics Dashboard API
**Tech Stack**: Express + PostgreSQL + Redis

- Data collection endpoints
- Aggregation queries
- Caching layer
- Report generation
- CSV exports
- Scheduled jobs

## Tech Stack Coverage

### Core Technologies
- **Node.js** (v18+)
- **JavaScript ES6+**
- **TypeScript** (optional)

### Frameworks & Libraries
- **Express.js** - Web framework
- **Fastify** - Alternative framework
- **Koa.js** - Modern framework

### Databases
- **MongoDB** with Mongoose
- **PostgreSQL** with pg/Sequelize
- **MySQL** with mysql2
- **Redis** for caching

### Authentication
- **jsonwebtoken** - JWT implementation
- **passport.js** - Authentication strategies
- **bcryptjs** - Password hashing

### Testing
- **Jest** - Test framework
- **Mocha** + **Chai** - Alternative stack
- **Supertest** - API testing
- **Sinon** - Mocking

### DevOps
- **Docker** - Containerization
- **PM2** - Process management
- **Nginx** - Reverse proxy
- **GitHub Actions** - CI/CD

### Utilities
- **dotenv** - Environment variables
- **helmet** - Security headers
- **cors** - Cross-origin requests
- **morgan** - HTTP logging
- **winston** - Application logging
- **express-validator** - Input validation

## Best Practices Included

- ✅ Clean code architecture (MVC/service layer)
- ✅ Error handling patterns
- ✅ Input validation
- ✅ Security best practices (OWASP)
- ✅ RESTful API design
- ✅ Database optimization
- ✅ Caching strategies
- ✅ Testing strategies (>80% coverage)
- ✅ Documentation (JSDoc, Swagger)
- ✅ Git workflow
- ✅ Environment-based configuration
- ✅ Logging and monitoring
- ✅ Code reviews
- ✅ Production deployment

## Plugin Structure

```
custom-plugin-nodejs/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
│
├── agents/                       # 7 Expert Agents
│   ├── 01-nodejs-fundamentals.md
│   ├── 02-express-framework.md
│   ├── 03-async-programming.md
│   ├── 04-database-integration.md
│   ├── 05-authentication-security.md
│   ├── 06-testing-debugging.md
│   └── 07-deployment-scaling.md
│
├── skills/                       # 7 Core Skills
│   ├── express-rest-api/SKILL.md
│   ├── async-patterns/SKILL.md
│   ├── mongoose-mongodb/SKILL.md
│   ├── jwt-authentication/SKILL.md
│   ├── jest-testing/SKILL.md
│   ├── docker-deployment/SKILL.md
│   └── performance-optimization/SKILL.md
│
├── commands/                     # 4 Quick-Start Commands
│   ├── start.md
│   ├── api.md
│   ├── test.md
│   └── deploy.md
│
└── README.md                     # This file
```

## Use Cases

### For Career Changers
- Learn backend development from scratch
- Build portfolio projects
- Prepare for Node.js interviews
- Transition to backend developer role

### For Beginners
- Step-by-step learning path
- Real-world project examples
- Best practices from day one
- Guided hands-on practice

### For Intermediate Developers
- Deepen Node.js knowledge
- Learn advanced patterns
- Master deployment and scaling
- Production-ready implementations

### For Senior Developers
- Architecture patterns
- Performance optimization
- Microservices design
- Mentoring and code review

## Resources & References

### Official Documentation
- [Node.js Official Docs](https://nodejs.org/docs)
- [Express.js Documentation](https://expressjs.com)
- [MongoDB Manual](https://docs.mongodb.com)
- [Jest Documentation](https://jestjs.io)
- [Docker Documentation](https://docs.docker.com)

### Learning Resources
- [Node.js Roadmap](https://roadmap.sh/nodejs)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [JavaScript.info](https://javascript.info)
- [MDN Web Docs](https://developer.mozilla.org)

### Community
- [Node.js Official Community](https://nodejs.org/en/community/)
- [Express.js GitHub](https://github.com/expressjs/express)
- [r/node on Reddit](https://reddit.com/r/node)
- [Stack Overflow - Node.js](https://stackoverflow.com/questions/tagged/node.js)

## Requirements

- Node.js 18+ (LTS recommended)
- npm 8+ or yarn 1.22+
- Basic JavaScript knowledge
- Code editor (VS Code recommended)
- Terminal/command line familiarity
- Claude Code (latest version)

## Version

- **Current**: 1.0.0
- **Release Date**: November 2024
- **Status**: Production Ready
- **Roadmap Based**: roadmap.sh/nodejs (November 2025)

## License

This plugin is provided for educational and professional development purposes.

## Contributing

Contributions, issues, and feature requests are welcome!
See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## Acknowledgments

- Based on the excellent [roadmap.sh Node.js path](https://roadmap.sh/nodejs)
- Inspired by the Node.js community
- Built for developers by developers

---

## Ready to Master Node.js Backend Development?

```bash
# Start your journey
/nodejs-start

# Build your first API
/nodejs-api

# Learn testing
/nodejs-test

# Deploy to production
/nodejs-deploy

# And become a Node.js backend expert! 🚀
```

**Happy Coding! 💻**

For questions, support, or feedback, visit the [GitHub repository](https://github.com/pluginagentmarketplace/custom-plugin-nodejs).
