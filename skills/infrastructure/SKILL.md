---
name: devops-cloud
description: Master DevOps practices including Docker, Kubernetes, CI/CD pipelines, Infrastructure as Code, and cloud platforms (AWS, GCP, Azure). Learn deployment and scaling.
---

# DevOps & Cloud Infrastructure Skill

Master modern infrastructure, containerization, orchestration, and cloud deployment strategies.

## Quick Start

DevOps fundamentals:
1. **Containerization** - Docker
2. **Orchestration** - Kubernetes
3. **CI/CD Pipelines** - Automated deployment
4. **Infrastructure as Code** - Terraform, Ansible
5. **Cloud Platforms** - AWS, GCP, Azure

## Containerization with Docker

### Why Containers?
```
Without Docker:
"Works on my machine" problem
Different environments cause issues
Deployment headaches

With Docker:
Package = Code + Dependencies + Config
Same environment everywhere
Reproducible, reliable
```

### Docker Basics

**Dockerfile**
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

**Docker Commands**
```bash
# Build image
docker build -t myapp:1.0 .

# Run container
docker run -p 3000:3000 myapp:1.0

# List containers
docker ps -a

# Push to registry
docker push myregistry/myapp:1.0

# Docker Compose (multiple containers)
docker-compose up -d
```

### Docker Best Practices
✅ Use specific base image versions
✅ Minimize layer count
✅ Order instructions by frequency change
✅ Use multi-stage builds
✅ Scan images for vulnerabilities
✅ Keep images small

## Container Orchestration with Kubernetes

### Kubernetes (K8s) Architecture
```
Cluster (Kubernetes Cluster)
├── Master Node
│   ├── API Server
│   ├── etcd (Database)
│   ├── Scheduler
│   └── Controller Manager
│
├── Worker Node 1
│   ├── Kubelet
│   ├── Container Runtime (Docker)
│   └── kube-proxy
│
└── Worker Node 2
    ├── Kubelet
    ├── Container Runtime
    └── kube-proxy
```

### Kubernetes Objects

**Pod** (Smallest deployable unit)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: myapp
    image: myapp:1.0
    ports:
    - containerPort: 3000
```

**Deployment** (Manages replicas)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:1.0
        ports:
        - containerPort: 3000
        resources:
          limits:
            cpu: 500m
            memory: 512Mi
```

**Service** (Network access)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
  type: LoadBalancer
```

### Kubernetes Commands
```bash
# Create resources
kubectl apply -f deployment.yaml

# Check status
kubectl get pods
kubectl get services
kubectl describe pod pod-name

# Scale deployment
kubectl scale deployment myapp --replicas=5

# Access logs
kubectl logs pod-name

# Port forward
kubectl port-forward service/myapp 3000:3000

# Delete resources
kubectl delete deployment myapp
```

## CI/CD Pipelines

### Pipeline Stages

```
Code → Build → Test → Deploy → Monitor
│
└─ Commit to Git
   ↓
   Run Tests (Unit, Integration)
   ↓
   Build Docker Image
   ↓
   Push to Registry
   ↓
   Deploy to Staging
   ↓
   Run E2E Tests
   ↓
   Deploy to Production
   ↓
   Monitor & Alert
```

### GitHub Actions Example
```yaml
name: Deploy Pipeline

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Run tests
      run: npm test

    - name: Build Docker image
      run: docker build -t myapp:${{ github.sha }} .

    - name: Push to registry
      run: docker push myapp:${{ github.sha }}

    - name: Deploy to Kubernetes
      run: |
        kubectl set image deployment/myapp \
          myapp=myapp:${{ github.sha }}
```

### Popular CI/CD Tools
- **GitHub Actions** - Built-in GitHub
- **GitLab CI** - GitLab integrated
- **Jenkins** - Self-hosted, powerful
- **CircleCI** - Cloud-based
- **Travis CI** - Simple, cloud

## Infrastructure as Code

### Terraform (AWS/Multi-cloud)
```hcl
# Provider
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

# EC2 Instance
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "web-server"
  }
}

# RDS Database
resource "aws_db_instance" "postgres" {
  allocated_storage    = 20
  engine               = "postgres"
  instance_class       = "db.t2.micro"
  name                 = "mydb"
  username             = "admin"
  password             = var.db_password
  skip_final_snapshot  = true
}

# Apply configuration
# terraform plan   - Preview changes
# terraform apply  - Apply changes
```

### Ansible (Configuration management)
```yaml
---
- hosts: webservers
  become: yes

  tasks:
  - name: Install Node.js
    apt:
      name: nodejs
      state: present

  - name: Install dependencies
    npm:
      name: "{{ item }}"
      path: /app
    loop:
      - express
      - dotenv

  - name: Start application
    systemd:
      name: myapp
      state: started
      enabled: yes
```

## Cloud Platforms

### AWS (Amazon Web Services)
**Most popular cloud platform**

Key services:
- **EC2** - Virtual machines
- **S3** - Object storage
- **RDS** - Managed databases
- **Lambda** - Serverless functions
- **ECS/EKS** - Container services
- **CloudFront** - CDN
- **Route 53** - DNS

Pricing: Pay as you go

### GCP (Google Cloud Platform)
**Strong in data & AI**

Key services:
- **Compute Engine** - VMs
- **Cloud Run** - Serverless
- **BigQuery** - Data warehouse
- **Cloud SQL** - Managed databases
- **Cloud Storage** - Object storage

Advantage: AI/ML tools, data analytics

### Azure (Microsoft Cloud)
**Strong in enterprise**

Key services:
- **Virtual Machines** - VMs
- **App Service** - Web hosting
- **SQL Database** - Managed databases
- **Functions** - Serverless
- **Cosmos DB** - NoSQL database

Advantage: Microsoft integration, Windows support

## Cloud Deployment Models

### Serverless
```
Benefits:
✅ No server management
✅ Auto-scaling
✅ Pay per execution
✅ Minimal operations

Challenges:
❌ Cold start latency
❌ Limited customization
❌ Vendor lock-in
```

### Containerized (K8s)
```
Benefits:
✅ Full control
✅ Efficient resource use
✅ Multi-cloud portable
✅ Scaling flexibility

Challenges:
❌ Complex to manage
❌ Requires expertise
❌ Operational overhead
```

### Traditional Servers (VMs)
```
Benefits:
✅ Full control
✅ Familiar
✅ Any framework
✅ Stateful applications

Challenges:
❌ Manual scaling
❌ Operational overhead
❌ Higher costs at scale
```

## Monitoring & Observability

### Key Metrics
```
Application
├── Response Time
├── Error Rate
├── Throughput (requests/sec)
└── Resource Usage

Infrastructure
├── CPU Usage
├── Memory Usage
├── Disk I/O
└── Network I/O

Business
├── Conversion Rate
├── User Engagement
└── Revenue Impact
```

### Popular Tools
- **Prometheus** - Metrics collection
- **Grafana** - Visualization
- **ELK Stack** - Logging (Elasticsearch, Logstash, Kibana)
- **Datadog** - Comprehensive monitoring
- **New Relic** - Application performance

### Alert Example
```yaml
- alert: HighErrorRate
  expr: rate(errors_total[5m]) > 0.05
  for: 5m
  annotations:
    summary: "High error rate detected"
    description: "Error rate is {{ $value | humanizePercentage }}"
```

## DevOps Best Practices

✅ **Infrastructure as Code** - Version control everything
✅ **Automated Testing** - Test before deployment
✅ **Continuous Integration** - Merge frequently
✅ **Continuous Deployment** - Deploy often, small changes
✅ **Monitoring** - Know what's happening
✅ **Alerts** - Get notified of issues
✅ **Logging** - Track what happened
✅ **Backups** - Always have recovery plan
✅ **Security** - Scan, harden, protect
✅ **Documentation** - Enable team knowledge

## Common DevOps Mistakes

- ❌ Complex infrastructure instead of simple
- ❌ No monitoring or logging
- ❌ Manual deployments
- ❌ Ignoring security
- ❌ No backup strategy
- ❌ Monolithic CI/CD pipelines
- ❌ Insufficient testing
- ❌ No disaster recovery plan

## Learning Path (4-6 months)

1. **Basics** (2 weeks)
   - Linux fundamentals
   - Git mastery
   - Bash scripting

2. **Containerization** (3 weeks)
   - Docker basics
   - Docker Compose
   - Image optimization

3. **Orchestration** (3 weeks)
   - Kubernetes basics
   - Deployments & Services
   - ConfigMaps & Secrets

4. **CI/CD** (3 weeks)
   - GitHub Actions/Jenkins
   - Pipeline design
   - Deployment strategies

5. **Cloud & IaC** (4 weeks)
   - Cloud platform (AWS/GCP)
   - Terraform/CloudFormation
   - Infrastructure design

6. **Monitoring** (2 weeks)
   - Metrics collection
   - Logging setup
   - Alerting strategy
