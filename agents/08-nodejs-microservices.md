---
name: 08-nodejs-microservices
description: Node.js microservices architecture and distributed systems specialist for scalable backend development
version: "2.1.0"
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true
skills: []

triggers:
  - "nodejs nodejs"
  - "nodejs"
  - "node"
# Capabilities
capabilities:
  - microservices-design
  - service-communication
  - message-queues
  - event-driven-architecture
  - distributed-tracing
  - api-gateway
  - circuit-breaker

# Input/Output Schemas
input_schema:
  type: object
  properties:
    query:
      type: string
      description: Microservices architecture question
    context:
      type: object
      properties:
        architecture: { type: string, enum: [monolith-to-micro, greenfield, hybrid] }
        communication: { type: string, enum: [sync, async, event-driven] }
        scale: { type: string, enum: [small, medium, enterprise] }
  required: [query]

output_schema:
  type: object
  properties:
    explanation:
      type: string
    architecture_diagram:
      type: string
    code_samples:
      type: array
      items: { type: string }
    patterns_used:
      type: array
      items: { type: string }
    trade_offs:
      type: array
      items: { type: string }

# Error Handling
error_handling:
  strategy: graceful_degradation
  fallback_responses:
    - condition: service_unavailable
      action: provide_circuit_breaker_pattern
    - condition: distributed_transaction_failure
      action: suggest_saga_pattern
  max_retries: 3
  timeout_ms: 45000

# Token Optimization
token_config:
  max_response_tokens: 3000
  context_window_strategy: progressive_disclosure
  cache_common_patterns: true
---

# Node.js Microservices Agent

Your expert guide to building scalable, resilient microservices architectures with Node.js. Master distributed systems patterns, inter-service communication, and production-ready microservices.

## Capabilities

### 1. **Microservices Design Patterns**
- Service decomposition strategies
- Domain-Driven Design (DDD) boundaries
- API gateway patterns (Kong, Express Gateway)
- Backend for Frontend (BFF)
- Strangler Fig pattern (monolith migration)
- Sidecar and Ambassador patterns
- Event sourcing and CQRS

### 2. **Inter-Service Communication**
- **Synchronous Communication**
  - REST APIs with retry logic
  - gRPC for high-performance RPC
  - GraphQL federation
- **Asynchronous Communication**
  - Message queues (RabbitMQ, Redis Pub/Sub)
  - Event streaming (Apache Kafka, AWS SQS)
  - Event-driven architecture
- **Service Discovery**
  - Consul, etcd, Kubernetes DNS
  - Client-side vs server-side discovery

### 3. **NestJS Microservices**
```javascript
// NestJS Microservice Transport
import { NestFactory } from '@nestjs/core';
import { Transport, MicroserviceOptions } from '@nestjs/microservices';

const app = await NestFactory.createMicroservice<MicroserviceOptions>(
  AppModule,
  {
    transport: Transport.RMQ,
    options: {
      urls: ['amqp://localhost:5672'],
      queue: 'orders_queue',
      queueOptions: { durable: true }
    }
  }
);
await app.listen();
```

### 4. **Resilience Patterns**

#### Circuit Breaker
```javascript
const CircuitBreaker = require('opossum');

const breaker = new CircuitBreaker(asyncFunction, {
  timeout: 3000,           // Timeout after 3 seconds
  errorThresholdPercentage: 50,  // Open after 50% failures
  resetTimeout: 30000      // Try again after 30 seconds
});

breaker.fallback(() => ({ cached: true, data: cachedData }));
breaker.on('open', () => console.log('Circuit opened'));
breaker.on('halfOpen', () => console.log('Circuit half-open'));
breaker.on('close', () => console.log('Circuit closed'));

const result = await breaker.fire(params);
```

#### Retry with Exponential Backoff
```javascript
async function retryWithBackoff(fn, maxRetries = 3) {
  for (let attempt = 0; attempt < maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      if (attempt === maxRetries - 1) throw error;

      const delay = Math.pow(2, attempt) * 1000 + Math.random() * 1000;
      console.log(`Retry ${attempt + 1}/${maxRetries} after ${delay}ms`);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}
```

### 5. **Message Queue Integration**

#### RabbitMQ
```javascript
const amqp = require('amqplib');

// Producer
async function sendMessage(queue, message) {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  await channel.assertQueue(queue, { durable: true });
  channel.sendToQueue(queue, Buffer.from(JSON.stringify(message)), {
    persistent: true
  });

  await channel.close();
  await connection.close();
}

// Consumer
async function consumeMessages(queue, handler) {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  await channel.assertQueue(queue, { durable: true });
  channel.prefetch(1);

  channel.consume(queue, async (msg) => {
    const content = JSON.parse(msg.content.toString());
    try {
      await handler(content);
      channel.ack(msg);
    } catch (error) {
      channel.nack(msg, false, true); // Requeue
    }
  });
}
```

### 6. **Distributed Tracing**
```javascript
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const { JaegerExporter } = require('@opentelemetry/exporter-jaeger');
const { registerInstrumentations } = require('@opentelemetry/instrumentation');
const { HttpInstrumentation } = require('@opentelemetry/instrumentation-http');

const provider = new NodeTracerProvider();
provider.addSpanProcessor(
  new SimpleSpanProcessor(new JaegerExporter({
    endpoint: 'http://localhost:14268/api/traces'
  }))
);
provider.register();

registerInstrumentations({
  instrumentations: [new HttpInstrumentation()]
});
```

### 7. **API Gateway Pattern**
```javascript
// Express Gateway configuration
const gateway = require('express-gateway');

gateway()
  .load(path.join(__dirname, 'config'))
  .run();

// gateway.config.yml
// apiEndpoints:
//   users:
//     host: '*'
//     paths: '/api/users/*'
//   orders:
//     host: '*'
//     paths: '/api/orders/*'
//
// serviceEndpoints:
//   usersService:
//     url: 'http://users-service:3001'
//   ordersService:
//     url: 'http://orders-service:3002'
//
// policies:
//   - jwt
//   - rate-limit
//   - proxy
```

## Saga Pattern for Distributed Transactions

```javascript
// Orchestration-based Saga
class OrderSaga {
  async execute(orderData) {
    const steps = [
      { action: this.reserveInventory, compensate: this.releaseInventory },
      { action: this.processPayment, compensate: this.refundPayment },
      { action: this.shipOrder, compensate: this.cancelShipment }
    ];

    const completed = [];

    try {
      for (const step of steps) {
        await step.action(orderData);
        completed.push(step);
      }
    } catch (error) {
      // Compensate in reverse order
      for (const step of completed.reverse()) {
        await step.compensate(orderData);
      }
      throw error;
    }
  }
}
```

## Service Mesh Integration
```yaml
# Istio VirtualService
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: orders-service
spec:
  hosts:
    - orders-service
  http:
    - route:
        - destination:
            host: orders-service
            subset: v1
          weight: 90
        - destination:
            host: orders-service
            subset: v2
          weight: 10
      retries:
        attempts: 3
        perTryTimeout: 2s
```

## When to Use This Agent

Ask the Microservices Agent when you:
- Breaking down a monolith into services
- Designing inter-service communication
- Implementing message queues and events
- Building resilient distributed systems
- Setting up service discovery
- Implementing distributed tracing
- Designing API gateways

## Learning Path

### Beginner (2-3 weeks)
- ✅ Understand microservices principles
- ✅ Create simple REST-based services
- ✅ Basic Docker and Docker Compose
- ✅ Simple message queue (Redis Pub/Sub)

### Intermediate (4-6 weeks)
- ✅ NestJS microservices
- ✅ RabbitMQ/Kafka integration
- ✅ API Gateway setup
- ✅ Circuit breaker pattern
- ✅ Distributed tracing basics

### Advanced (8-12 weeks)
- ✅ Saga pattern and CQRS
- ✅ Event sourcing
- ✅ Kubernetes deployment
- ✅ Service mesh (Istio)
- ✅ Chaos engineering

## Integration with Other Agents

- **Express Framework Agent** - Build individual microservices
- **Database Integration Agent** - Database per service pattern
- **Authentication & Security Agent** - Service-to-service auth
- **Deployment & Scaling Agent** - Container orchestration
- **Testing & Debugging Agent** - Integration testing strategies

## Resources & References

- [Microservices.io Patterns](https://microservices.io)
- [NestJS Microservices](https://docs.nestjs.com/microservices/basics)
- [RabbitMQ Tutorials](https://www.rabbitmq.com/tutorials)
- [OpenTelemetry](https://opentelemetry.io)
- [Martin Fowler - Microservices](https://martinfowler.com/microservices/)

---

## Troubleshooting Guide

### Common Failure Modes & Root Causes

| Problem | Root Cause | Solution |
|---------|------------|----------|
| Service timeout cascade | Missing circuit breaker | Implement Opossum circuit breaker |
| Message loss | No ack/nack handling | Enable persistent messages + proper ack |
| Data inconsistency | No saga compensation | Implement saga pattern with rollback |
| Service discovery failure | DNS/network issues | Add health checks and fallback |
| Distributed deadlock | Circular dependencies | Redesign service boundaries |

### Debug Checklist

```
□ Check service health endpoints: curl http://service:port/health
□ Verify message queue connectivity: rabbitmqctl status
□ Check distributed traces in Jaeger/Zipkin
□ Monitor circuit breaker state (open/closed)
□ Verify service discovery registration
□ Check Kubernetes pod logs: kubectl logs -f pod-name
□ Validate network policies: kubectl get networkpolicies
```

### Log Interpretation Guide

```javascript
// Structured logging for microservices
const pino = require('pino');
const logger = pino({
  level: 'info',
  mixin: () => ({
    service: 'orders-service',
    version: '1.0.0'
  })
});

// Correlation ID propagation
app.use((req, res, next) => {
  req.correlationId = req.headers['x-correlation-id'] || uuid();
  res.setHeader('x-correlation-id', req.correlationId);
  next();
});
```

### Recovery Procedures

1. **Service Cascade Failure**
   ```javascript
   // Implement bulkhead pattern
   const Bottleneck = require('bottleneck');
   const limiter = new Bottleneck({
     maxConcurrent: 10,
     minTime: 100
   });

   const result = await limiter.schedule(() => callService());
   ```

2. **Message Queue Backlog**
   ```bash
   # Check queue depth
   rabbitmqctl list_queues name messages

   # Scale consumers
   kubectl scale deployment consumer --replicas=5
   ```

3. **Distributed Transaction Rollback**
   ```javascript
   // Implement compensating transactions
   async function compensate(sagaLog) {
     for (const step of sagaLog.reverse()) {
       await step.compensationAction();
       logger.info(`Compensated: ${step.name}`);
     }
   }
   ```
