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
  channel: string; // varchar(64), default "web"
  subject?: string;
  initialMessage: string;
}
// Note: channel is a loose string (not a union) to support
// dynamic plugin registration of new channels (e.g. WhatsApp, Telegram).

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
// PluginManagerService handles discovery and loading automatically.
// No manual configuration needed.
```

#### 3. Fail Fast, Fail Loud

Catch errors early and provide actionable feedback.

**In practice:**

- Strict TypeScript configuration
- Schema validation on all inputs
- Comprehensive error types
- Detailed error messages

```typescript
// tRPC procedures validate inputs with Zod schemas.
// Invalid input is rejected automatically with clear, structured errors.
const createUser = protectedProcedure
  .input(z.object({
    email: z.string().email("Invalid email format"),
    name: z.string().min(1),
  }))
  .mutation(async ({ input }) => { ... });
```

#### 4. Plugins for External Integrations

Core features (conversations, analytics, agents) are regular services and routes in the server codebase. Plugins extend Hay with external integrations only.

**Why:**

- Keeps the core simple and cohesive
- Plugins can be added or removed without touching core code
- External services have their own lifecycle and configuration

**In practice:**

```typescript
// Core features are services/routes in /server
// e.g. ConversationService, AnalyticsService, AgentService

// Plugins handle external integrations
// e.g. plugins/stripe, plugins/zendesk, plugins/whatsapp
// Loaded dynamically by PluginManagerService from /plugins directory
```

#### 5. Data Ownership and Privacy

Users own their data, always.

**In practice:**

- Export functionality for all data
- Clear data retention policies
- Encryption for sensitive plugin configuration
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

#### Hook-Driven Architecture

**Why:** Decouples components and enables real-time features

```typescript
import { hookManager } from "@server/services/hooks/hook-manager";

// Trigger hooks for major state changes
await hookManager.trigger("conversation.resolved", {
  organizationId,
  conversationId,
  metadata: { resolvedBy },
});

// Register handlers to react to events
hookManager.register("conversation.resolved", async (payload) => {
  await sendSurvey(payload.conversationId);
});
```

#### Direct Service Construction

Services construct their own dependencies via `new` in constructors or use singletons (e.g. `HookManager.getInstance()`). This keeps the codebase simple and avoids the overhead of a DI framework.

```typescript
// Services self-construct dependencies
class ConversationService {
  private repository = new ConversationRepository();
  private hookManager = HookManager.getInstance();
}
```

#### Repository Pattern

**Why:** Abstracts data access with a consistent base class

```typescript
// All repositories extend BaseRepository<T>
export class ConversationRepository extends BaseRepository<Conversation> {
  constructor() {
    super(Conversation);
  }

  async findByChannel(channel: string): Promise<Conversation[]> {
    return this.getRepository().find({ where: { channel } });
  }
}
```

### Code Quality Standards

#### TypeScript Usage

- **Strict mode enabled**: No implicit any
- **Explicit return types**: For all public functions
- **Discriminated unions**: For state management
- **Plain string IDs**: All entity IDs are plain `string` (UUIDs)

```typescript
// IDs are plain strings (UUIDs generated by the database)
interface Conversation {
  id: string;
  customerId: string;
  channel: string;
}
```

#### Testing

- **Unit tests**: For business logic
- **Integration tests**: For API endpoints
- **E2E tests**: For critical user flows
- **Good coverage encouraged**: No hard threshold configured

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

- **TSDoc comments**: Encouraged on public APIs (not consistently enforced)
- **README in each package**: Setup and usage
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
