<div align="center">

<!-- Animated Typing Banner -->
<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=28&duration=3000&pause=1000&color=2E9EF7&center=true&vCenter=true&multiline=true&repeat=true&width=600&height=100&lines=Nodejs+Assistant;8+Agents+%7C+12+Skills;Claude+Code+Plugin" alt="Nodejs Assistant" />

<br/>

<!-- Badge Row 1: Status Badges -->
[![Version](https://img.shields.io/badge/Version-2.0.0-blue?style=for-the-badge)](https://github.com/pluginagentmarketplace/custom-plugin-nodejs/releases)
[![License](https://img.shields.io/badge/License-Custom-yellow?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Production-brightgreen?style=for-the-badge)](#)
[![SASMP](https://img.shields.io/badge/SASMP-v1.3.0-blueviolet?style=for-the-badge)](#)

<!-- Badge Row 2: Content Badges -->
[![Agents](https://img.shields.io/badge/Agents-8-orange?style=flat-square&logo=robot)](#-agents)
[![Skills](https://img.shields.io/badge/Skills-12-purple?style=flat-square&logo=lightning)](#-skills)
[![Commands](https://img.shields.io/badge/Commands-4-green?style=flat-square&logo=terminal)](#-commands)

<br/>

<!-- Quick CTA Row -->
[📦 **Install Now**](#-quick-start) · [🤖 **Explore Agents**](#-agents) · [📖 **Documentation**](#-documentation) · [⭐ **Star this repo**](https://github.com/pluginagentmarketplace/custom-plugin-nodejs)

---

### What is this?

> **Nodejs Assistant** is a Claude Code plugin with **8 agents** and **12 skills** for nodejs development.

</div>

---

## 📑 Table of Contents

<details>
<summary>Click to expand</summary>

- [Quick Start](#-quick-start)
- [Features](#-features)
- [Agents](#-agents)
- [Skills](#-skills)
- [Commands](#-commands)
- [Documentation](#-documentation)
- [Contributing](#-contributing)
- [License](#-license)

</details>

---

## 🚀 Quick Start

### Prerequisites

- Claude Code CLI v2.0.27+
- Active Claude subscription

### Installation (Choose One)

<details open>
<summary><strong>Option 1: From Marketplace (Recommended)</strong></summary>

```bash
# Step 1️⃣ Add the marketplace
/plugin marketplace add pluginagentmarketplace/custom-plugin-nodejs

# Step 2️⃣ Install the plugin
/plugin install nodejs-developer-plugin@pluginagentmarketplace-nodejs

# Step 3️⃣ Restart Claude Code
# Close and reopen your terminal/IDE
```

</details>

<details>
<summary><strong>Option 2: Local Installation</strong></summary>

```bash
# Clone the repository
git clone https://github.com/pluginagentmarketplace/custom-plugin-nodejs.git
cd custom-plugin-nodejs

# Load locally
/plugin load .

# Restart Claude Code
```

</details>

### ✅ Verify Installation

After restart, you should see these agents:

```
nodejs-developer-plugin:07-deployment-scaling
nodejs-developer-plugin:05-authentication-security
nodejs-developer-plugin:06-testing-debugging
nodejs-developer-plugin:04-database-integration
nodejs-developer-plugin:03-async-programming
... and 2 more
```

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🤖 **8 Agents** | Specialized AI agents for nodejs tasks |
| 🛠️ **12 Skills** | Reusable capabilities with Golden Format |
| ⌨️ **4 Commands** | Quick slash commands |
| 🔄 **SASMP v1.3.0** | Full protocol compliance |

---

## 🤖 Agents

### 8 Specialized Agents

| # | Agent | Purpose |
|---|-------|---------|
| 1 | **07-deployment-scaling** | Deploy and scale Node.js applications with Docker, PM2, clus |
| 2 | **05-authentication-security** | Implement secure authentication and authorization in Node.js |
| 3 | **06-testing-debugging** | Master testing and debugging Node.js applications with Jest, |
| 4 | **04-database-integration** | Connect Node.js applications to databases including MongoDB, |
| 5 | **03-async-programming** | Master asynchronous programming in Node.js with Promises, as |
| 6 | **02-express-framework** | Build production-ready REST APIs with Express.js including r |
| 7 | **01-nodejs-fundamentals** | Master Node.js core concepts including event loop, modules,  |

---

## 🛠️ Skills

### Available Skills

| Skill | Description | Invoke |
|-------|-------------|--------|
| `async-patterns` | Master asynchronous programming in Node.js with Promises, as | `Skill("nodejs-developer-plugin:async-patterns")` |
| `express-rest-api` | Build production-ready RESTful APIs with Express.js includin | `Skill("nodejs-developer-plugin:express-rest-api")` |
| `docker-deployment` | Containerize and deploy Node.js applications with Docker inc | `Skill("nodejs-developer-plugin:docker-deployment")` |
| `jest-testing` | Test Node.js applications with Jest including unit tests, in | `Skill("nodejs-developer-plugin:jest-testing")` |
| `jwt-authentication` | Implement secure JWT (JSON Web Token) authentication in Node | `Skill("nodejs-developer-plugin:jwt-authentication")` |
| `performance-optimization` | Optimize Node.js application performance with caching, clust | `Skill("nodejs-developer-plugin:performance-optimization")` |
| `mongoose-mongodb` | Work with MongoDB in Node.js using Mongoose ODM for schema d | `Skill("nodejs-developer-plugin:mongoose-mongodb")` |

---

## ⌨️ Commands

| Command | Description |
|---------|-------------|
| `/explore` | /explore |
| `/assess` | /assess |
| `/projects` | /projects |
| `/plan` | /plan |

---

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [CHANGELOG.md](CHANGELOG.md) | Version history |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to contribute |
| [LICENSE](LICENSE) | License information |

---

## 📁 Project Structure

<details>
<summary>Click to expand</summary>

```
custom-plugin-nodejs/
├── 📁 .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── 📁 agents/              # 8 agents
├── 📁 skills/              # 12 skills (Golden Format)
├── 📁 commands/            # 4 commands
├── 📁 hooks/
├── 📄 README.md
├── 📄 CHANGELOG.md
└── 📄 LICENSE
```

</details>

---

## 📅 Metadata

| Field | Value |
|-------|-------|
| **Version** | 2.0.0 |
| **Last Updated** | 2025-12-29 |
| **Status** | Production Ready |
| **SASMP** | v1.3.0 |
| **Agents** | 8 |
| **Skills** | 12 |
| **Commands** | 4 |

---

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guide](CONTRIBUTING.md).

1. Fork the repository
2. Create your feature branch
3. Follow the Golden Format for new skills
4. Submit a pull request

---

## ⚠️ Security

> **Important:** This repository contains third-party code and dependencies.
>
> - ✅ Always review code before using in production
> - ✅ Check dependencies for known vulnerabilities
> - ✅ Follow security best practices
> - ✅ Report security issues privately via [Issues](../../issues)

---

## 📝 License

Copyright © 2025 **Dr. Umit Kacar** & **Muhsin Elcicek**

Custom License - See [LICENSE](LICENSE) for details.

---

## 👥 Contributors

<table>
<tr>
<td align="center">
<strong>Dr. Umit Kacar</strong><br/>
Senior AI Researcher & Engineer
</td>
<td align="center">
<strong>Muhsin Elcicek</strong><br/>
Senior Software Architect
</td>
</tr>
</table>

---

<div align="center">

**Made with ❤️ for the Claude Code Community**

[![GitHub](https://img.shields.io/badge/GitHub-pluginagentmarketplace-black?style=for-the-badge&logo=github)](https://github.com/pluginagentmarketplace)

</div>
