---
layout: docs.njk
title: Philosophy
description: The principles and values that guide Hay's development
section: technical
navGroup: Introduction
navOrder: 3
---

## Development Philosophy

Hay is built on a set of core principles that guide every technical decision. Understanding these principles will help you contribute effectively and build plugins that align with Hay's vision.

### Core Principles

#### 1. Developer Experience First

We believe that happy developers build better software.

**In practice:**

- Comprehensive TypeScript types throughout
- Clear, self-documenting code
- Detailed error messages with debugging hints
- Hot reload in development
- Extensive tooling and scripts

```typescript
// Good: Clear, typed schema (Zod)
const createConversationSchema = z.object({
  title: z.string().min(1).max(255).optional(),
  language: z.string().max(35).optional(),
  metadata: z.record(z.string(), z.unknown()).optional(),
  status: settableStatusSchema.optional(),
  customerId: z.string().uuid().optional(),
  playbookVersionOverride: z
    .object({ playbookId: z.string().uuid(), versionId: z.string().uuid() })
    .optional(),
});

// Bad: Unclear, any types
function create(params: any) { ... }
```

#### 2. Convention Over Configuration

Sensible defaults that work out of the box.

**In practice:**

- Zero config to get started
- Environment-based configuration
- Automatic discovery of plugins
- Intelligent defaults that can be overridden

```typescript
// Core plugins are auto-discovered from plugins/core/ at startup via PluginManagerService.discoverPlugins()
// Custom plugins are uploaded via the plugin upload service to plugins/custom/{orgId}/
```

#### 3. Fail Fast, Fail Loud

Catch errors early and provide actionable feedback.

**In practice:**

- Strict TypeScript configuration
- Schema validation on all inputs
- Comprehensive error types
- Detailed error messages

```typescript
// Validation errors are clear and actionable
throw new TRPCError({ code: 'BAD_REQUEST', message: 'Invalid email format' });
```

#### 4. Plugin-First Architecture

External integrations are always plugins, never bolted directly onto core logic. Core features (conversations, agents, orchestration, documents) live in the server codebase, but all third-party service integrations are implemented as plugins.

**Why:**

- Forces modular design for integrations
- Ensures extensibility
- Dogfooding our own plugin APIs
- Easy to add/remove integrations

**In practice:**

Core plugins are auto-discovered from `plugins/core/` at startup. Org-specific plugins are uploaded via the plugin upload service to `plugins/custom/{organizationId}/`. The core source code never hardcodes plugin IDs.

#### 5. Data Ownership and Privacy

Users own their data, always.

**In practice:**

- Export functionality for all data
- Clear data retention policies
- Encryption by default
- GDPR compliance built-in
- Self-hosting option available

#### 6. Performance Matters

Fast software is better software.

**Optimization strategy:**

1. Measure first (no premature optimization)
2. Optimize the critical path
3. Cache aggressively
4. Load lazily

```typescript
// Example: Lazy loading plugins
const plugin = await import("./plugins/heavy-feature");
if (userNeedsFeature) {
  await plugin.init();
}
```

### Design Patterns

#### Event-Driven Architecture

**Why:** Decouples components and enables real-time features

Events are emitted via `ConversationEventsService` and WebSocket service, not a general event bus.

#### Dependency Injection

**Why:** Testability and flexibility

Services use singleton exports and direct module imports (e.g., `export const myService = new MyService()`).

#### Repository Pattern

**Why:** Abstracts data access, easy to swap databases

Repositories extend `BaseRepository<T>` (a generic TypeORM wrapper) — no interface abstraction layer.

### Code Quality Standards

#### TypeScript Usage

- **Strict mode enabled**: No implicit any
- **Explicit return types**: For all public functions
- **Discriminated unions**: For state management
- **Branded types**: For IDs and sensitive data (aspirational pattern — not yet implemented in the codebase; IDs are currently plain strings)

#### Testing

- **Unit tests**: For business logic
- **Integration tests**: For API endpoints
- **E2E tests**: For critical user flows
- **Minimum 80% coverage**: For new code (aspirational target — not currently enforced via CI)

```typescript
describe("DataRetentionService", () => {
  it("should anonymize conversations past the retention window", async () => {
    const service = setupService();
    const org = createTestOrg({ retentionDays: 30 });

    const result = await service.anonymizeExpired(org);

    expect(result.anonymizedCount).toBeGreaterThan(0);
  });
});
```

#### Documentation

- **TSDoc comments**: On all public APIs
- **README in each package**: Setup and usage
- **Architecture Decision Records**: For major decisions (planned — not yet adopted)
- **Inline comments**: Only for "why", not "what"

````typescript
/**
 * Processes a conversation through the orchestrator pipeline.
 *
 * The trigger (customer_message, inactivity, etc.) comes from the
 * RabbitMQ queue message, not as a parameter. The method acquires
 * a lock internally and skips processing if the lock is held.
 *
 * @param conversationId - The ID of the conversation to process
 *
 * @example
 * ```ts
 * await orchestrator.processConversation(conversationId);
 * ```
 */
async processConversation(conversationId: string): Promise<void>;
````

### Contribution Guidelines

When contributing to Hay:

1. **Follow these principles**: They're not just guidelines
2. **Write tests**: Code without tests won't be merged
3. **Update docs**: Code and docs should be in sync
4. **Start small**: Small PRs are reviewed faster
5. **Ask questions**: Better to ask than assume

### Learning Resources

- **[Architecture Guide](/docs/technical/architecture/)** - System design details
- **[Plugin Development](/docs/technical/plugins/getting-started/)** - Build your first plugin
- **[Contributing Guide](/docs/technical/)** - How to contribute

## Next Steps

Ready to build? Start with:

- [Setting up your development environment](/docs/technical/)
- [Creating your first plugin](/docs/technical/plugins/getting-started/)
- [Understanding the architecture](/docs/technical/architecture/)
