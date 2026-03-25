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
// Good: Clear, typed interface
interface CreateConversationParams {
  customerId: string;
  channel: 'email' | 'chat' | 'social';
  subject?: string;
  initialMessage: string;
}

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
// Convention: Plugins auto-discovered from /plugins directory
// Configuration: Override defaults only when needed
{
  pluginDirectory: './custom-plugins',  // optional
  autoDiscovery: true                    // default
}
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
throw new ValidationError({
  field: "email",
  message: "Invalid email format",
  received: userInput,
  expected: "user@example.com",
});
```

#### 4. Plugin-First Architecture

Everything is a plugin, including core features.

**Why:**

- Forces modular design
- Ensures extensibility
- Dogfooding our own APIs
- Easy to add/remove features

**In practice:**

```typescript
// Core features implemented as plugins
const corePlugins = ["hay-plugin-conversations", "hay-plugin-automation", "hay-plugin-analytics"];

// User plugins loaded the same way
const userPlugins = ["@custom/slack-integration", "@custom/ai-responses"];
```

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

```typescript
// Emit events for major state changes
eventBus.emit("conversation.resolved", {
  conversationId,
  resolvedBy,
  timestamp,
});

// Plugins can react to any event
plugin.on("conversation.resolved", async (event) => {
  await sendSurvey(event.conversationId);
});
```

#### Dependency Injection

**Why:** Testability and flexibility

```typescript
// Services injected, not imported
class AutomationService {
  constructor(
    private db: Database,
    private queue: Queue,
    private events: EventBus
  ) {}
}

// Easy to mock in tests
const mockDb = createMockDatabase();
const service = new AutomationService(mockDb, ...);
```

#### Repository Pattern

**Why:** Abstracts data access, easy to swap databases

```typescript
interface ConversationRepository {
  findById(id: string): Promise<Conversation>;
  create(data: CreateConversationData): Promise<Conversation>;
  update(id: string, data: Partial<Conversation>): Promise<Conversation>;
}

// PostgreSQL implementation
class PostgresConversationRepository implements ConversationRepository {
  // ...
}

// Could swap to MongoDB, etc.
class MongoConversationRepository implements ConversationRepository {
  // ...
}
```

### Code Quality Standards

#### TypeScript Usage

- **Strict mode enabled**: No implicit any
- **Explicit return types**: For all public functions
- **Discriminated unions**: For state management
- **Branded types**: For IDs and sensitive data

```typescript
// Branded type prevents mixing IDs
type ConversationId = string & { __brand: 'ConversationId' };
type UserId = string & { __brand: 'UserId' };

// Compile error: can't pass UserId where ConversationId expected
function getConversation(id: ConversationId) { ... }
```

#### Testing

- **Unit tests**: For business logic
- **Integration tests**: For API endpoints
- **E2E tests**: For critical user flows
- **Minimum 80% coverage**: For new code

```typescript
describe("AutomationService", () => {
  it("should trigger rule when conditions match", async () => {
    const service = setupService();
    const rule = createTestRule();

    const result = await service.evaluate(rule, testConversation);

    expect(result.triggered).toBe(true);
  });
});
```

#### Documentation

- **TSDoc comments**: On all public APIs
- **README in each package**: Setup and usage
- **Architecture Decision Records**: For major decisions
- **Inline comments**: Only for "why", not "what"

````typescript
/**
 * Evaluates automation rules against a conversation.
 *
 * @param rule - The automation rule to evaluate
 * @param conversation - The conversation context
 * @returns Evaluation result with actions to execute
 *
 * @throws {ValidationError} If rule or conversation is invalid
 *
 * @example
 * ```ts
 * const result = await evaluate(rule, conversation);
 * if (result.matched) {
 *   await executeActions(result.actions);
 * }
 * ```
 */
async function evaluate(
  rule: AutomationRule,
  conversation: Conversation,
): Promise<EvaluationResult>;
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
