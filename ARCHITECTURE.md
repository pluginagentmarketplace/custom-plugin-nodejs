# Node.js Developer Plugin - Architecture Guide

Production-grade Node.js backend development plugin with 8 agents and 12 skills.

## System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                 Node.js Developer Plugin v2.1.0                  │
│                      SASMP v1.3.0 | EQHM Enabled                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                    Commands (4)                             │ │
│  │  /assess | /explore | /plan | /projects                    │ │
│  └────────────────────────────────────────────────────────────┘ │
│                            │                                     │
│                            ▼                                     │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                    Agents (8)                               │ │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐          │ │
│  │  │01-Fundamentals│ │02-Express  │ │03-Async    │          │ │
│  │  └─────────────┘ └─────────────┘ └─────────────┘          │ │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐          │ │
│  │  │04-Database  │ │05-Auth/Sec │ │06-Testing   │          │ │
│  │  └─────────────┘ └─────────────┘ └─────────────┘          │ │
│  │  ┌─────────────┐ ┌─────────────┐                          │ │
│  │  │07-Deploy    │ │08-Microserv│                          │ │
│  │  └─────────────┘ └─────────────┘                          │ │
│  └────────────────────────────────────────────────────────────┘ │
│                            │                                     │
│                            ▼                                     │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                    Skills (12)                              │ │
│  │  async-patterns | docker-deployment | error-handling        │ │
│  │  express-rest-api | graphql | jest-testing                  │ │
│  │  jwt-authentication | mongoose-mongodb | nestjs             │ │
│  │  performance-optimization | streams | websockets            │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Dependency Graph

```
                    Agent → Skill Bond Map
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

01-nodejs-fundamentals
    ├── async-patterns (PRIMARY)
    ├── docker-deployment (PRIMARY)
    ├── express-rest-api (PRIMARY)
    ├── jest-testing (PRIMARY)
    ├── jwt-authentication (PRIMARY)
    ├── mongoose-mongodb (PRIMARY)
    └── performance-optimization (PRIMARY)

02-express-framework
    └── graphql (PRIMARY)

03-async-programming
    └── streams (PRIMARY)

06-testing-debugging
    └── error-handling (PRIMARY)

08-nodejs-microservices
    ├── nestjs (PRIMARY)
    └── websockets (PRIMARY)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Agent Capabilities Matrix

| Agent | Version | Capabilities | Token Config |
|-------|---------|--------------|--------------|
| 01-nodejs-fundamentals | 2.1.0 | event-loop, modules, npm, async, error-handling | 2000 |
| 02-express-framework | 2.1.0 | routing, middleware, REST, security, versioning | 2500 |
| 03-async-programming | 2.1.0 | promises, async/await, streams, events, concurrency | 2000 |
| 04-database-integration | 2.1.0 | mongodb, postgresql, mysql, orms, migrations | 2500 |
| 05-authentication-security | 2.1.0 | jwt, oauth, sessions, rbac, csrf | 2000 |
| 06-testing-debugging | 2.1.0 | jest, mocha, debugging, integration, e2e | 2500 |
| 07-deployment-scaling | 2.1.0 | docker, pm2, clustering, k8s, ci/cd | 3000 |
| 08-nodejs-microservices | 2.1.0 | design, communication, queues, tracing, gateway | 3000 |

## Skill Structure

Each skill follows SASMP v1.3.0 format:

```yaml
---
name: skill-name
description: Concise skill description
version: "2.1.0"
sasmp_version: "1.3.0"
bonded_agent: agent-name
bond_type: PRIMARY_BOND
---

# Skill Title

## Quick Start (4 steps)
## Core Concepts (code examples)
## Learning Path (Beginner → Intermediate → Advanced)
## Troubleshooting (table format)
## Unit Test Template
## Related Skills
## Resources
```

## Production-Grade Features

### Error Handling Strategy
```yaml
error_handling:
  strategy: graceful_degradation | strict_security
  fallback_responses:
    - condition: specific_error_type
      action: specific_action
  max_retries: 1-3
  timeout_ms: 20000-60000
```

### Token Optimization
```yaml
token_config:
  max_response_tokens: 2000-3000
  context_window_strategy: progressive_disclosure
  cache_common_patterns: true
```

### Input/Output Schemas
Each agent defines type-safe:
- `input_schema`: Required query + optional context
- `output_schema`: Structured response format

## Integrity Validation

### Validation Checklist
- [x] All 8 agents in plugin.json
- [x] All 12 skills in plugin.json
- [x] All 4 commands in plugin.json
- [x] Zero orphan skills (all bonded)
- [x] Zero missing bonds
- [x] Zero circular dependencies
- [x] Version consistency (2.1.0)
- [x] SASMP version match (1.3.0)

### Skill Bond Validation
```
✓ async-patterns → 01-nodejs-fundamentals
✓ docker-deployment → 01-nodejs-fundamentals
✓ error-handling → 06-testing-debugging
✓ express-rest-api → 01-nodejs-fundamentals
✓ graphql → 02-express-framework
✓ jest-testing → 01-nodejs-fundamentals
✓ jwt-authentication → 01-nodejs-fundamentals
✓ mongoose-mongodb → 01-nodejs-fundamentals
✓ nestjs → 08-nodejs-microservices
✓ performance-optimization → 01-nodejs-fundamentals
✓ streams → 03-async-programming
✓ websockets → 08-nodejs-microservices
```

## Quality Standards

### Ethical
- No dark patterns in examples
- Security warnings for risky operations
- OWASP considerations included

### Honest
- Accurate capability claims
- Clear limitations stated
- Trade-offs documented

### Modern
- 2024-2025 best practices
- Current library versions
- Production-tested patterns

### Maintainable
- Self-documenting code examples
- Consistent structure
- Clear troubleshooting guides

---

## Changelog

### v2.1.0 (2025-01-01) - Production-Grade Update
**Added:**
- 08-nodejs-microservices agent (full implementation)
- 5 new skills: error-handling, graphql, nestjs, streams, websockets
- Input/Output schemas for all agents
- Error handling patterns with fallback strategies
- Token optimization configs
- Troubleshooting sections for all agents
- Unit test templates for all skills
- Dependency graph visualization
- Integrity validation metadata

**Changed:**
- All agents upgraded to v2.1.0 with enhanced frontmatter
- All skills upgraded to v2.1.0 with production patterns
- plugin.json includes all 8 agents and 12 skills
- marketplace.json updated with correct counts
- Skill descriptions enhanced with code examples

**Fixed:**
- Missing agent (08-nodejs-microservices) added to plugin.json
- Missing skills added to plugin.json
- Bond type corrections (streams → 03-async-programming)

### v2.0.0 (Previous) - EQHM Compliance
- Initial SASMP v1.3.0 compliance
- 7 agents, 7 skills structure

### v1.0.0 (Initial)
- Basic Node.js plugin structure
