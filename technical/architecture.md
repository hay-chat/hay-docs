---
layout: docs.njk
title: Architecture
description: Understanding Hay's system architecture and design decisions
section: technical
navGroup: Introduction
navOrder: 2
---

## System Architecture

Hay is designed as a modular, event-driven platform that scales with your needs. This document explains the key architectural decisions and how components work together.

### High-Level Overview

```mermaid
graph LR
  Clients["fa:fa-globe Clients<br/>Web / Mobile"]
  Gateway["fa:fa-shield-halved API Gateway<br/>Express"]
  Services["fa:fa-puzzle-piece Services<br/>Plugins"]
  Queue["fa:fa-list-check Message Queue<br/>RabbitMQ + Redis"]
  DB["fa:fa-database Database<br/>PostgreSQL"]

  Clients --> Gateway --> Services
  Gateway --> Queue
  Services --> DB

  style Clients fill:#e8f3ff,stroke:#568aff,color:#0a155c
  style Gateway fill:#e8f3ff,stroke:#568aff,color:#0a155c
  style Services fill:#e8f3ff,stroke:#568aff,color:#0a155c
  style Queue fill:#f5f5f5,stroke:#d4d4d4,color:#404040
  style DB fill:#f5f5f5,stroke:#d4d4d4,color:#404040
```

### Core Components

#### 1. API Gateway

The entry point for all client requests:

- **Authentication**: JWT-based auth with refresh tokens
- **Rate Limiting**: Prevents abuse and ensures fair usage
- **Request Validation**: Schema validation using Zod
- **Response Formatting**: Consistent API responses

#### 2. Service Layer

Business logic organized as modular services:

- **Conversation Service**: Manages conversations and messages
- **Playbook Service**: Handles playbook-based workflows and automation
- **Plugin Manager Service**: Connects to external platforms via plugin instances
- **Analytics Service**: Processes and stores metrics

#### 3. Plugin System

Hay's extensibility mechanism:

Plugins are defined using `defineHayPlugin()` from `@hay/plugin-sdk`. The plugin contract is `HayPluginManifest` in `/server/types/plugin-sdk.types.ts`, exposing capabilities via a runtime `/metadata` endpoint and interacting with the platform via `HayGlobalContext`.

Each plugin can:
- Register event listeners
- Extend the API
- Add new UI components
- Access core services

#### 4. Real-Time & Background Messaging

There is no single event bus module. Real-time events are published to Redis pub/sub channels via `ConversationEventsService`. Background processing tasks are queued via RabbitMQ (`rabbitmqService`) and a Redis-backed `JobQueueService`.

#### 5. Data Layer

Persistent storage with caching:

- **PostgreSQL**: Primary data store for conversations, users, settings
- **Redis**: Caching layer and pub/sub for real-time features
- **File Storage (local filesystem or optional S3)**: Attachments and media files

### Data Flow

#### Incoming Message

1. Webhook arrives at the plugin's registered route (e.g., `/v1/plugins/:pluginId/webhook`)
2. Plugin normalizes the message and stores it in the database
3. Orchestrator processes the message via RabbitMQ (perception, retrieval, execution layers)
4. Response is delivered via Redis pub/sub
5. WebSocket pushes real-time updates to connected clients

#### Workflow Automation

Playbooks are the workflow mechanism in Hay. They define structured sequences of actions (e.g., collect information, route conversations, trigger plugin tools) that the orchestrator can execute during conversations. There is no standalone rules engine; all automation flows through playbook definitions.

### Scalability Considerations

#### Horizontal Scaling

Hay is designed to scale horizontally:

- **Stateless API servers**: Scale by adding more instances
- **Background workers**: Scale job processing independently
- **Database read replicas**: Future scalability option (not currently implemented)

#### Caching Strategy

Multi-layer caching reduces database load:

```mermaid
graph LR
  A["fa:fa-browser Client Cache"] --> C["fa:fa-bolt Redis"] --> D["fa:fa-database Database"]

  style A fill:#e8f3ff,stroke:#568aff,color:#0a155c
  style C fill:#e8f3ff,stroke:#568aff,color:#0a155c
  style D fill:#f5f5f5,stroke:#d4d4d4,color:#404040
```

- **Client**: Browser cache for static assets
- **Redis**: In-memory cache for hot data
- **Database**: Source of truth

#### Message Queue

RabbitMQ (via `amqplib`) handles orchestrator messaging, and a custom Postgres-backed job queue (using `SKIP LOCKED`) coordinated through Redis pub/sub handles background jobs:

- Failed jobs are marked as `FAILED` by `JobQueueService.failJob()`; there is no automatic retry
- Job priority is ordered via a SQL column, not a Bull-style priority queue
- No rate limiting per job type
- Monitoring and alerting

### Security Architecture

#### Defense in Depth

Multiple security layers:

1. **Network**: TLS/SSL encryption for all traffic
2. **Authentication**: JWT with configurable expiration (default: 7 days)
3. **Authorization**: Role-based access control (RBAC)
4. **Input Validation**: Sanitize all user input
5. **Output Encoding**: Prevent XSS attacks
6. **Database**: Prepared statements prevent SQL injection

#### Data Privacy

- **Audit logs**: Specific operations (authentication, data export, deletion) are logged for compliance purposes

### Monitoring and Observability

#### Metrics

Application-level metrics are not yet instrumented. The following are planned for future implementation:

- API response times
- Error rates
- Queue lengths
- Database query performance
- Cache hit rates

#### Logging

Structured logging via Pino:

```typescript
logger.info({ conversationId: '123', action: 'create', duration: 45 }, 'Processing conversation');
```


## Next Steps

- Learn about our [development philosophy](/docs/technical/philosophy/)
- Start [building plugins](/docs/technical/plugins/getting-started/)
- Explore the [API reference](/docs/technical/plugins/api-reference/)
