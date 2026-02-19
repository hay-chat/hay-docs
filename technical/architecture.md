---
layout: docs.njk
title: System Architecture Overview
description: Understanding Hay's system architecture and design decisions
section: technical
---

## System Architecture

Hay is designed as a modular, event-driven platform that scales with your needs. This document explains the key architectural decisions and how components work together.

### High-Level Overview

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Clients   │────▶│   API Gateway │────▶│  Services   │
│  (Web/Mobile)│     │   (Express)   │     │  (Plugins)  │
└─────────────┘     └─────────────┘     └─────────────┘
                             │                    │
                             ▼                    ▼
                    ┌─────────────┐     ┌─────────────┐
                    │   Message   │     │  Database   │
                    │    Queue    │     │ (PostgreSQL)│
                    │   (Bull)    │     └─────────────┘
                    └─────────────┘
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
- **Automation Service**: Handles rules and workflows
- **Integration Service**: Connects to external platforms
- **Analytics Service**: Processes and stores metrics

#### 3. Plugin System

Hay's extensibility mechanism:

```typescript
interface Plugin {
  name: string;
  version: string;
  init: (context: PluginContext) => Promise<void>;
  hooks: {
    [eventName: string]: HookFunction;
  };
}
```

Each plugin can:
- Register event listeners
- Extend the API
- Add new UI components
- Access core services

#### 4. Event Bus

Central communication hub:

```typescript
// Emit an event
eventBus.emit('conversation.created', {
  conversationId: '123',
  channel: 'email',
  customer: { ... }
});

// Listen for events
eventBus.on('conversation.created', async (event) => {
  // Handle the event
});
```

Events flow through the system triggering:
- Automation rules
- Real-time updates
- Analytics tracking
- Plugin hooks

#### 5. Data Layer

Persistent storage with caching:

- **PostgreSQL**: Primary data store for conversations, users, settings
- **Redis**: Caching layer and pub/sub for real-time features
- **Object Storage**: Attachments and media files

### Data Flow

#### Incoming Message

1. Message arrives via integration (email, chat, etc.)
2. Integration plugin emits `message.received` event
3. Message is validated and stored in database
4. Event bus notifies all listeners
5. Automation rules are evaluated
6. Real-time updates sent to connected clients

#### Automation Execution

1. Rule trigger conditions evaluated
2. If matched, rule added to job queue
3. Worker picks up job from queue
4. Actions executed in sequence
5. Results logged and stored
6. Completion event emitted

### Scalability Considerations

#### Horizontal Scaling

Hay is designed to scale horizontally:

- **Stateless API servers**: Scale by adding more instances
- **Background workers**: Scale job processing independently
- **Database read replicas**: Distribute read load

#### Caching Strategy

Multi-layer caching reduces database load:

```
Client Cache → CDN → Redis → Database
```

- **Client**: Browser cache for static assets
- **CDN**: Edge caching for global delivery
- **Redis**: In-memory cache for hot data
- **Database**: Source of truth

#### Message Queue

Bull queue for reliable background processing:

- Retry failed jobs automatically
- Priority queues for urgent tasks
- Rate limiting per job type
- Monitoring and alerting

### Security Architecture

#### Defense in Depth

Multiple security layers:

1. **Network**: TLS/SSL encryption for all traffic
2. **Authentication**: JWT with short expiration
3. **Authorization**: Role-based access control (RBAC)
4. **Input Validation**: Sanitize all user input
5. **Output Encoding**: Prevent XSS attacks
6. **Database**: Prepared statements prevent SQL injection

#### Data Privacy

- **Encryption at rest**: Database encryption enabled
- **Encryption in transit**: TLS 1.3 required
- **PII handling**: Separate tables with restricted access
- **Audit logs**: All data access logged

### Monitoring and Observability

#### Metrics

Key metrics tracked:

- API response times
- Error rates
- Queue lengths
- Database query performance
- Cache hit rates

#### Logging

Structured logging with correlation IDs:

```typescript
logger.info('Processing conversation', {
  correlationId: req.id,
  conversationId: '123',
  action: 'create',
  duration: 45
});
```

#### Tracing

Distributed tracing for debugging:
- Request flows across services
- Performance bottleneck identification
- Error source pinpointing

## Next Steps

- Learn about our [development philosophy](/docs/technical/philosophy/)
- Start [building plugins](/docs/technical/plugins/getting-started/)
- Explore the [API reference](/docs/technical/api/authentication/)
