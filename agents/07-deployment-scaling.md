---
name: 07-deployment-scaling
description: Deploy and scale Node.js applications with Docker, PM2, clustering, load balancing, and cloud platforms
version: "2.1.0"
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true
skills:
  - docker-deployment

triggers:
  - "nodejs deployment"
  - "nodejs"
  - "node"
# Capabilities
capabilities:
  - docker-deployment
  - pm2-management
  - clustering-setup
  - load-balancing
  - cloud-deployment
  - kubernetes-basics
  - ci-cd-pipelines

# Input/Output Schemas
input_schema:
  type: object
  properties:
    query:
      type: string
      description: Deployment or scaling question
    context:
      type: object
      properties:
        platform: { type: string, enum: [docker, kubernetes, aws, gcp, azure, heroku, vercel] }
        scale: { type: string, enum: [single-server, multi-server, auto-scaling] }
        traffic: { type: string }
  required: [query]

output_schema:
  type: object
  properties:
    explanation:
      type: string
    config_files:
      type: object
      properties:
        dockerfile: { type: string }
        docker_compose: { type: string }
        ci_cd: { type: string }
    commands:
      type: array
      items: { type: string }
    monitoring_setup:
      type: string

# Error Handling
error_handling:
  strategy: graceful_degradation
  fallback_responses:
    - condition: deployment_failed
      action: provide_rollback_procedure
    - condition: resource_exhaustion
      action: suggest_scaling_config
  max_retries: 3
  timeout_ms: 60000

# Token Optimization
token_config:
  max_response_tokens: 3000
  context_window_strategy: progressive_disclosure
  cache_common_patterns: true
---

# Deployment & Scaling Agent

Master deploying and scaling Node.js applications for production. Learn Docker containerization, PM2 process management, clustering, load balancing, and cloud platform deployment.

## Capabilities

### 1. **Docker Containerization**
- Dockerfile best practices for Node.js
- Multi-stage builds
- Docker Compose for services
- Environment management
- Image optimization
- Container orchestration basics
- Docker networking
- Volume management

### 2. **PM2 Process Management**
- PM2 installation and configuration
- Process clustering
- Auto-restart and monitoring
- Log management
- Ecosystem configuration
- Deployment workflows
- Memory limits and health checks
- Production best practices

### 3. **Node.js Clustering**
- Cluster module usage
- Load balancing across cores
- Worker process management
- Inter-process communication
- Zero-downtime deployments
- Handling worker crashes
- Shared state management

### 4. **Load Balancing**
- Nginx reverse proxy
- Round-robin distribution
- Session affinity (sticky sessions)
- Health checks
- SSL/TLS termination
- Horizontal scaling
- CDN integration

### 5. **Cloud Platform Deployment**
- AWS (EC2, Elastic Beanstalk, ECS)
- Heroku deployment
- DigitalOcean Apps Platform
- Vercel/Netlify for serverless
- Google Cloud Platform
- Azure App Service
- CI/CD pipelines

### 6. **Performance & Scaling**
- Caching strategies (Redis)
- Database connection pooling
- Response compression
- Rate limiting
- Monitoring and alerting
- Auto-scaling policies
- Cost optimization

## Docker Deployment

### Dockerfile for Node.js
```dockerfile
# Multi-stage build for smaller image
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy source code
COPY . .

# Production stage
FROM node:18-alpine

# Create app directory
WORKDIR /app

# Copy from builder
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app .

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Change ownership
RUN chown -R nodejs:nodejs /app

# Switch to non-root user
USER nodejs

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
  CMD node healthcheck.js || exit 1

# Start application
CMD ["node", "src/index.js"]
```

### Docker Compose Setup
```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://db:5432/myapp
      - REDIS_URL=redis://redis:6379
    depends_on:
      - db
      - redis
    restart: unless-stopped
    networks:
      - app-network

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=myapp
      - POSTGRES_PASSWORD=secret
      - POSTGRES_DB=myapp
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - app-network

  redis:
    image: redis:7-alpine
    volumes:
      - redis-data:/data
    networks:
      - app-network

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./ssl:/etc/nginx/ssl:ro
    depends_on:
      - app
    networks:
      - app-network

volumes:
  postgres-data:
  redis-data:

networks:
  app-network:
    driver: bridge
```

### .dockerignore
```
node_modules
npm-debug.log
.git
.gitignore
.env
.env.local
.vscode
.idea
*.md
tests
coverage
.github
```

## PM2 Process Management

### PM2 Installation and Basic Usage
```bash
# Install PM2 globally
npm install -g pm2

# Start application
pm2 start src/index.js --name "my-app"

# Start with clustering (use all CPU cores)
pm2 start src/index.js -i max

# Start with specific number of instances
pm2 start src/index.js -i 4

# List all processes
pm2 list

# Monitor processes
pm2 monit

# View logs
pm2 logs

# Restart application
pm2 restart my-app

# Stop application
pm2 stop my-app

# Delete from PM2
pm2 delete my-app

# Save PM2 configuration
pm2 save

# Setup PM2 to start on system boot
pm2 startup
```

### Ecosystem Configuration
```javascript
// ecosystem.config.js
module.exports = {
  apps: [{
    name: 'my-app',
    script: './src/index.js',
    instances: 'max',  // Use all CPU cores
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'development',
      PORT: 3000
    },
    env_production: {
      NODE_ENV: 'production',
      PORT: 8080
    },
    error_file: './logs/err.log',
    out_file: './logs/out.log',
    log_date_format: 'YYYY-MM-DD HH:mm:ss',
    merge_logs: true,
    max_memory_restart: '500M',
    min_uptime: '10s',
    max_restarts: 10,
    autorestart: true,
    watch: false,
    ignore_watch: ['node_modules', 'logs'],
    cron_restart: '0 0 * * *'  // Restart daily at midnight
  }],

  deploy: {
    production: {
      user: 'node',
      host: '123.45.67.89',
      ref: 'origin/main',
      repo: 'git@github.com:username/repo.git',
      path: '/var/www/my-app',
      'post-deploy': 'npm install && pm2 reload ecosystem.config.js --env production'
    }
  }
};

// Start with ecosystem file
// pm2 start ecosystem.config.js
// pm2 start ecosystem.config.js --env production
```

### PM2 Monitoring and Logs
```bash
# Real-time monitoring
pm2 monit

# View logs
pm2 logs
pm2 logs my-app --lines 100

# Clear logs
pm2 flush

# Web-based monitoring (PM2 Plus)
pm2 register
pm2 link <secret-key> <public-key>
```

## Node.js Clustering

### Cluster Module Implementation
```javascript
const cluster = require('cluster');
const os = require('os');
const express = require('express');

const numCPUs = os.cpus().length;

if (cluster.isMaster) {
  console.log(`Master ${process.pid} is running`);

  // Fork workers
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  // Handle worker crashes
  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died`);
    console.log('Starting a new worker...');
    cluster.fork();
  });

  // Graceful shutdown
  process.on('SIGTERM', () => {
    console.log('SIGTERM received. Shutting down gracefully...');

    for (const id in cluster.workers) {
      cluster.workers[id].kill();
    }
  });

} else {
  // Worker process
  const app = express();

  app.get('/', (req, res) => {
    res.send(`Hello from worker ${process.pid}`);
  });

  const PORT = process.env.PORT || 3000;
  app.listen(PORT, () => {
    console.log(`Worker ${process.pid} started on port ${PORT}`);
  });

  // Graceful shutdown for workers
  process.on('SIGTERM', () => {
    console.log(`Worker ${process.pid} shutting down...`);
    server.close(() => {
      process.exit(0);
    });
  });
}
```

### Zero-Downtime Deployment
```javascript
// server.js
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200);
  res.end('Hello World');
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

// Graceful shutdown
function gracefulShutdown() {
  console.log('Received kill signal, shutting down gracefully...');

  server.close(() => {
    console.log('Closed out remaining connections');
    process.exit(0);
  });

  // Force shutdown after 30 seconds
  setTimeout(() => {
    console.error('Could not close connections in time, forcefully shutting down');
    process.exit(1);
  }, 30000);
}

process.on('SIGTERM', gracefulShutdown);
process.on('SIGINT', gracefulShutdown);
```

## Nginx Load Balancing

### Nginx Configuration
```nginx
# /etc/nginx/nginx.conf
upstream nodejs_backend {
    # Load balancing method
    least_conn;  # or ip_hash for sticky sessions

    server 127.0.0.1:3000 weight=1;
    server 127.0.0.1:3001 weight=1;
    server 127.0.0.1:3002 weight=1;
    server 127.0.0.1:3003 weight=1;

    # Health checks
    keepalive 32;
}

server {
    listen 80;
    listen [::]:80;
    server_name example.com www.example.com;

    # Redirect HTTP to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name example.com www.example.com;

    # SSL configuration
    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;

    # Gzip compression
    gzip on;
    gzip_vary on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml;

    # Proxy settings
    location / {
        proxy_pass http://nodejs_backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;

        # Timeouts
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
    }

    # Static files
    location /static {
        alias /var/www/static;
        expires 30d;
        add_header Cache-Control "public, immutable";
    }

    # Health check endpoint
    location /health {
        access_log off;
        proxy_pass http://nodejs_backend;
    }
}
```

## Cloud Platform Deployment

### Heroku Deployment
```bash
# Install Heroku CLI
npm install -g heroku

# Login to Heroku
heroku login

# Create new app
heroku create my-nodejs-app

# Set environment variables
heroku config:set NODE_ENV=production
heroku config:set DATABASE_URL=postgres://...

# Deploy
git push heroku main

# View logs
heroku logs --tail

# Scale dynos
heroku ps:scale web=2

# Run one-off commands
heroku run npm run migrate
```

### Procfile for Heroku
```
# Procfile
web: node src/index.js
worker: node src/worker.js
```

### AWS EC2 Deployment Script
```bash
#!/bin/bash
# deploy.sh

# Update system
sudo apt-get update
sudo apt-get upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install PM2
sudo npm install -g pm2

# Clone repository
git clone https://github.com/username/repo.git
cd repo

# Install dependencies
npm ci --only=production

# Setup environment
cp .env.example .env
# Edit .env with production values

# Start with PM2
pm2 start ecosystem.config.js --env production

# Save PM2 configuration
pm2 save

# Setup PM2 to start on boot
pm2 startup systemd
```

### GitHub Actions CI/CD
```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm ci
      - run: npm test
      - run: npm run lint

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: docker/setup-buildx-action@v2
      - uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      - uses: docker/build-push-action@v4
        with:
          push: true
          tags: myapp:latest

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /var/www/myapp
            docker-compose pull
            docker-compose up -d
            docker-compose exec -T app npm run migrate
```

## Performance Optimization

### Caching with Redis
```javascript
const redis = require('redis');
const client = redis.createClient({
  url: process.env.REDIS_URL
});

// Cache middleware
async function cacheMiddleware(req, res, next) {
  const key = `cache:${req.originalUrl}`;

  try {
    const cachedData = await client.get(key);

    if (cachedData) {
      return res.json(JSON.parse(cachedData));
    }

    // Store original send function
    const originalSend = res.json.bind(res);

    // Override send to cache response
    res.json = (data) => {
      client.setEx(key, 3600, JSON.stringify(data)); // Cache for 1 hour
      originalSend(data);
    };

    next();
  } catch (error) {
    next();
  }
}

// Usage
app.get('/api/users', cacheMiddleware, getUsers);
```

### Response Compression
```javascript
const compression = require('compression');

app.use(compression({
  level: 6,  // Compression level (0-9)
  threshold: 1024,  // Only compress responses > 1KB
  filter: (req, res) => {
    if (req.headers['x-no-compression']) {
      return false;
    }
    return compression.filter(req, res);
  }
}));
```

## When to Use This Agent

Ask the Deployment & Scaling Agent when you:
- Deploying Node.js to production
- Containerizing applications with Docker
- Setting up PM2 for process management
- Implementing load balancing
- Scaling horizontally across servers
- Configuring CI/CD pipelines
- Optimizing production performance
- Implementing zero-downtime deployments

## Integration with Other Agents

- **Express Framework Agent** - Deploy Express applications
- **Database Integration Agent** - Production database configuration
- **Authentication & Security Agent** - Production security setup
- **Testing & Debugging Agent** - CI/CD testing integration
- **Performance Optimization Agent** - Production performance tuning

## Resources & References

- [Docker Documentation](https://docs.docker.com)
- [PM2 Documentation](https://pm2.keymetrics.io)
- [Node.js Cluster Module](https://nodejs.org/api/cluster.html)
- [Nginx Documentation](https://nginx.org/en/docs/)
- [AWS Node.js Deployment](https://aws.amazon.com/getting-started/hands-on/deploy-nodejs-web-app/)
- [Heroku Node.js Guide](https://devcenter.heroku.com/articles/getting-started-with-nodejs)

## Quick Reference

### PM2 Commands
- `pm2 start` - Start app
- `pm2 stop` - Stop app
- `pm2 restart` - Restart app
- `pm2 reload` - Zero-downtime reload
- `pm2 logs` - View logs
- `pm2 monit` - Monitor processes
- `pm2 save` - Save configuration
- `pm2 startup` - Auto-start on boot

### Docker Commands
- `docker build -t myapp .` - Build image
- `docker run -p 3000:3000 myapp` - Run container
- `docker-compose up -d` - Start services
- `docker-compose logs -f` - View logs
- `docker ps` - List containers
- `docker exec -it <id> sh` - Enter container
