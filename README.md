<div align="center">

# Node.js Developer Plugin

### Complete Node.js Backend Mastery for Claude Code

**Master Express REST APIs, MongoDB, JWT authentication, testing, and production deployment with 7 specialized agents and 7 production-ready skills**

[![Verified](https://img.shields.io/badge/Verified-Working-success?style=flat-square&logo=checkmarx)](https://github.com/pluginagentmarketplace/custom-plugin-nodejs)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)
[![Version](https://img.shields.io/badge/Version-2.0.0-blue?style=flat-square)](https://github.com/pluginagentmarketplace/custom-plugin-nodejs)
[![Status](https://img.shields.io/badge/Status-Production_Ready-brightgreen?style=flat-square)](https://github.com/pluginagentmarketplace/custom-plugin-nodejs)
[![Agents](https://img.shields.io/badge/Agents-7-orange?style=flat-square)](#agents-overview)
[![Skills](https://img.shields.io/badge/Skills-7-purple?style=flat-square)](#skills-reference)
[![SASMP](https://img.shields.io/badge/SASMP-v1.3.0-blueviolet?style=flat-square)](#)

[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](skills/async-patterns/)
[![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)](skills/express-rest-api/)
[![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](skills/mongoose-mongodb/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](skills/docker-deployment/)

[Quick Start](#quick-start) | [Agents](#agents-overview) | [Skills](#skills-reference) | [Commands](#commands)

</div>

---

## Verified Installation

> **This plugin has been tested and verified working on Claude Code.**
> Last verified: December 2025

---

## Quick Start

### Option 1: Install from GitHub (Recommended)

```bash
# Step 1: Add the marketplace from GitHub
/plugin add marketplace pluginagentmarketplace/custom-plugin-nodejs

# Step 2: Install the plugin
/plugin install nodejs-developer-plugin@pluginagentmarketplace-nodejs

# Step 3: Restart Claude Code to load new plugins
```

### Option 2: Clone and Load Locally

```bash
# Clone the repository
git clone https://github.com/pluginagentmarketplace/custom-plugin-nodejs.git

# Navigate to the directory in Claude Code
cd custom-plugin-nodejs

# Load the plugin
/plugin load .
```

After loading, restart Claude Code.

### Verify Installation

After restarting Claude Code, verify the plugin is loaded. You should see these agents available:

```
custom-plugin-nodejs:01-nodejs-fundamentals
custom-plugin-nodejs:02-express-framework
custom-plugin-nodejs:03-async-programming
custom-plugin-nodejs:04-database-integration
custom-plugin-nodejs:05-authentication-security
custom-plugin-nodejs:06-testing-debugging
custom-plugin-nodejs:07-deployment-scaling
```

---

## Available Skills

Once installed, these 7 skills become available:

| Skill | Invoke Command | Description |
|-------|----------------|-------------|
| Async Patterns | `Skill("nodejs-developer-plugin:async-patterns")` | Promises, async/await, streams |
| Docker Deployment | `Skill("nodejs-developer-plugin:docker-deployment")` | Containerization, PM2, cloud |
| Express REST API | `Skill("nodejs-developer-plugin:express-rest-api")` | Routes, middleware, validation |
| Jest Testing | `Skill("nodejs-developer-plugin:jest-testing")` | Unit tests, Supertest, coverage |
| JWT Authentication | `Skill("nodejs-developer-plugin:jwt-authentication")` | Tokens, OAuth, security |
| Mongoose MongoDB | `Skill("nodejs-developer-plugin:mongoose-mongodb")` | Schema, CRUD, aggregation |
| Performance Optimization | `Skill("nodejs-developer-plugin:performance-optimization")` | Caching, clustering, profiling |

---

## What This Plugin Does

This plugin provides **7 specialized agents** and **7 production-ready skills** for complete Node.js backend mastery:

| Agent | Purpose |
|-------|---------|
| **Node.js Fundamentals** | Event loop, modules, npm, async programming |
| **Express Framework** | REST APIs, routing, middleware, error handling |
| **Async Programming** | Promises, async/await, streams, event-driven |
| **Database Integration** | MongoDB, PostgreSQL, MySQL, ORMs |
| **Authentication Security** | JWT, OAuth, sessions, security best practices |
| **Testing Debugging** | Jest, Mocha, Supertest, debugging techniques |
| **Deployment Scaling** | Docker, PM2, clustering, load balancing |

---

## Agents Overview

### 7 Implementation Agents

Each agent is designed to **do the work**, not just explain:

| Agent | Capabilities | Example Prompts |
|-------|--------------|-----------------|
| **Fundamentals** | Event loop, modules, npm | `"Explain event loop"`, `"Setup npm project"` |
| **Express** | REST APIs, middleware | `"Create Express API"`, `"Add middleware"` |
| **Async** | Promises, streams, events | `"Async/await patterns"`, `"Stream processing"` |
| **Database** | MongoDB, PostgreSQL, ORMs | `"Setup Mongoose"`, `"Prisma schema"` |
| **Auth Security** | JWT, OAuth, RBAC | `"JWT authentication"`, `"OAuth2 flow"` |
| **Testing** | Jest, Supertest, coverage | `"Write Jest tests"`, `"API testing"` |
| **Deployment** | Docker, PM2, cloud | `"Dockerize app"`, `"PM2 setup"` |

---

## Commands

4 interactive commands for Node.js development workflows:

| Command | Usage | Description |
|---------|-------|-------------|
| `/start` | `/start` | Get started with Node.js development |
| `/api` | `/api` | Build REST API with Express + MongoDB |
| `/test` | `/test` | Setup testing with Jest |
| `/deploy` | `/deploy` | Deploy to production with Docker |

---

## Skills Reference

Each skill includes **Golden Format** content:
- `assets/` - Configuration templates and setup files
- `scripts/` - Automation and validation scripts
- `references/` - Methodology guides and best practices

### All 7 Skills by Category

| Category | Skills |
|----------|--------|
| **Core** | async-patterns |
| **API** | express-rest-api |
| **Database** | mongoose-mongodb |
| **Security** | jwt-authentication |
| **Testing** | jest-testing |
| **DevOps** | docker-deployment |
| **Performance** | performance-optimization |

---

## Usage Examples

### Example 1: Create Express REST API

```javascript
// Before: No backend

// After (with Express Framework agent):
Skill("nodejs-developer-plugin:express-rest-api")

// Generates:
// - Express server setup
// - Route structure (MVC)
// - Middleware configuration
// - Error handling
```

### Example 2: Setup MongoDB with Mongoose

```javascript
// Before: Manual database setup

// After (with Database agent):
Skill("nodejs-developer-plugin:mongoose-mongodb")

// Provides:
// - Schema definitions
// - Model configuration
// - CRUD operations
// - Aggregation pipelines
```

### Example 3: Implement JWT Authentication

```javascript
// Before: No authentication

// After (with Auth Security agent):
Skill("nodejs-developer-plugin:jwt-authentication")

// Creates:
// - Token generation
// - Refresh token flow
// - Protected routes
// - RBAC implementation
```

---

## Plugin Structure

```
custom-plugin-nodejs/
├── .claude-plugin/
│   ├── plugin.json           # Plugin manifest
│   └── marketplace.json      # Marketplace config
├── agents/                   # 7 specialized agents
│   ├── 01-nodejs-fundamentals.md
│   ├── 02-express-framework.md
│   ├── 03-async-programming.md
│   ├── 04-database-integration.md
│   ├── 05-authentication-security.md
│   ├── 06-testing-debugging.md
│   └── 07-deployment-scaling.md
├── skills/                   # 7 skills (Golden Format)
│   ├── async-patterns/SKILL.md
│   ├── docker-deployment/SKILL.md
│   ├── express-rest-api/SKILL.md
│   ├── jest-testing/SKILL.md
│   ├── jwt-authentication/SKILL.md
│   ├── mongoose-mongodb/SKILL.md
│   └── performance-optimization/SKILL.md
├── commands/                 # 4 slash commands
│   ├── api.md
│   ├── deploy.md
│   ├── start.md
│   └── test.md
├── hooks/hooks.json
├── README.md
├── CHANGELOG.md
└── LICENSE
```

---

## Technology Coverage

| Category | Technologies |
|----------|--------------|
| **Runtime** | Node.js 18+, npm, yarn |
| **Framework** | Express.js, Fastify, Koa |
| **Database** | MongoDB, PostgreSQL, MySQL, Redis |
| **ORM** | Mongoose, Sequelize, Prisma |
| **Auth** | JWT, Passport.js, bcrypt, OAuth 2.0 |
| **Testing** | Jest, Mocha, Supertest, Sinon |
| **DevOps** | Docker, PM2, Nginx, GitHub Actions |
| **Security** | Helmet, CORS, Rate Limiting |

---

## Learning Paths

| Path | Duration | Focus |
|------|----------|-------|
| Beginner | 3-4 months | Fundamentals, REST APIs, MongoDB |
| Intermediate | 5-6 months | Advanced patterns, microservices |
| Advanced | 6-12 months | Architecture, scaling, optimization |

### Recommended Sequence
1. Node.js Fundamentals
2. Express Framework
3. Async Programming
4. Database Integration
5. Authentication Security
6. Testing Debugging
7. Deployment Scaling

---

## Requirements

| Requirement | Version |
|-------------|---------|
| Node.js | 18+ (LTS) |
| npm | 8+ |
| Docker | 20+ (optional) |
| MongoDB | 6+ (for database skills) |

---

## Metadata

| Field | Value |
|-------|-------|
| **Last Updated** | 2025-12-28 |
| **Maintenance Status** | Active |
| **SASMP Version** | 1.3.0 |
| **Support** | [Issues](../../issues) |

---

## License

MIT License - See [LICENSE](LICENSE) for details.

---

## Contributing

Contributions are welcome:

1. Fork the repository
2. Create a feature branch
3. Follow the Golden Format for new skills
4. Submit a pull request

---

## Contributors

**Authors:**
- **Dr. Umit Kacar** - Senior AI Researcher & Engineer
- **Muhsin Elcicek** - Senior Software Architect

---

<div align="center">

**Master Node.js backend development with AI assistance!**

[![Made for Node.js](https://img.shields.io/badge/Made%20for-Node.js%20Developers-339933?style=for-the-badge&logo=nodedotjs)](https://github.com/pluginagentmarketplace/custom-plugin-nodejs)

**Built by Dr. Umit Kacar & Muhsin Elcicek**

*Based on [roadmap.sh/nodejs](https://roadmap.sh/nodejs)*

</div>
