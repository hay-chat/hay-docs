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
  channel: string; // varchar(64), defaults to "web"
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

Plugins are automatically discovered from the `plugins/` directory at startup. Each plugin contains a `manifest.json` that defines its configuration. No additional discovery configuration is needed.

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

Everything is a plugin, including core features.

**Why:**

- Forces modular design
- Ensures extensibility
- Dogfooding our own APIs
- Easy to add/remove features

**In practice:**

Plugins are discovered dynamically from the `plugins/` directory. The core source code never hardcodes plugin IDs.

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

#### Module-Level Singletons

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
describe("PlaybookService", () => {
  it("should match playbook when conditions apply", async () => {
    const service = setupService();
    const playbook = createTestPlaybook();

    const result = await service.evaluate(playbook, testConversation);

    expect(result.matched).toBe(true);
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
 * Evaluates a playbook against a conversation context.
 *
 * @param playbook - The playbook to evaluate
 * @param conversation - The conversation context
 * @returns Result indicating whether the playbook matched and actions to execute
 *
 * @throws {ValidationError} If playbook or conversation is invalid
 *
 * @example
 * ```ts
 * const result = await evaluate(playbook, conversation);
 * if (result.matched) {
 *   await executeActions(result.actions);
 * }
 * ```
 */
async function evaluate(
  playbook: Playbook,
  conversation: Conversation,
): Promise<PlaybookEvaluationResult>;
````

### Contribution Guidelines

When contributing to Hay:

1. **Follow these principles**: They're not just guidelines
2. **Write tests**: Code without tests won't be merged
3. **Update docs**: Code and docs should be in sync
4. **Start small**: Small PRs are reviewed faster
5. **Ask questions**: Better to ask than assume

### Learning Resources

- **[Architecture Guide](/docs/technical/architecture)** - System design details
- **[Plugin Development](/docs/technical/plugins/getting-started/)** - Build your first plugin
- **[Contributing Guide](/docs/technical/)** - How to contribute

## Next Steps

Ready to build? Start with:

- [Setting up your development environment](/docs/technical/)
- [Creating your first plugin](/docs/technical/plugins/getting-started/)
- [Understanding the architecture](/docs/technical/architecture/)
