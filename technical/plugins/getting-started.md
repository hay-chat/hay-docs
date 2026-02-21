---
layout: docs.njk
title: Getting Started
description: Build your first Hay plugin in under 30 minutes
section: technical
navGroup: Plugin Development
navOrder: 1
---

## Building Your First Plugin

Hay's plugin system allows you to extend and customize the platform. This guide will walk you through creating your first plugin.

### What You'll Build

We'll create a simple plugin that:
- Listens for new conversations
- Detects the customer's timezone
- Automatically tags the conversation with the timezone

### Prerequisites

Before starting, ensure you have:
- Node.js 18+ installed
- A Hay development environment set up
- Basic TypeScript knowledge

If you haven't set up your dev environment, see the [technical overview](/docs/technical/).

### Step 1: Create Plugin Structure

First, create a new directory for your plugin:

```bash
mkdir -p plugins/timezone-tagger
cd plugins/timezone-tagger
```

Initialize a new npm package:

```bash
npm init -y
```

Install required dependencies:

```bash
npm install @hay/plugin-sdk zod
npm install -D typescript @types/node
```

### Step 2: Define Plugin Manifest

Create `package.json` with plugin metadata:

```json
{
  "name": "@hay/plugin-timezone-tagger",
  "version": "1.0.0",
  "description": "Automatically tags conversations with customer timezone",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "hay": {
    "plugin": true,
    "requires": ["@hay/plugin-conversations"]
  },
  "scripts": {
    "build": "tsc",
    "dev": "tsc --watch"
  }
}
```

### Step 3: Configure TypeScript

Create `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "declaration": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

### Step 4: Write the Plugin

Create `src/index.ts`:

```typescript
import { Plugin, PluginContext } from '@hay/plugin-sdk';
import { z } from 'zod';

// Configuration schema
const ConfigSchema = z.object({
  enabled: z.boolean().default(true),
  tagPrefix: z.string().default('timezone:')
});

type Config = z.infer<typeof ConfigSchema>;

// Plugin implementation
export default class TimezonePlugin implements Plugin {
  name = '@hay/plugin-timezone-tagger';
  version = '1.0.0';

  private config: Config;

  constructor(config: unknown) {
    // Validate configuration
    this.config = ConfigSchema.parse(config);
  }

  async init(context: PluginContext): Promise<void> {
    // Register event listeners
    context.events.on('conversation.created',
      this.handleNewConversation.bind(this)
    );

    context.logger.info('Timezone tagger plugin initialized');
  }

  private async handleNewConversation(event: any): Promise<void> {
    if (!this.config.enabled) return;

    const { conversation, context } = event;

    // Detect timezone from customer data
    const timezone = await this.detectTimezone(conversation);

    if (timezone) {
      // Add timezone tag
      const tag = `${this.config.tagPrefix}${timezone}`;
      await context.services.conversations.addTag(
        conversation.id,
        tag
      );

      context.logger.info('Tagged conversation with timezone', {
        conversationId: conversation.id,
        timezone
      });
    }
  }

  private async detectTimezone(conversation: any): Promise<string | null> {
    // Try to get timezone from customer metadata
    if (conversation.customer?.timezone) {
      return conversation.customer.timezone;
    }

    // Try to detect from IP address
    if (conversation.customer?.ipAddress) {
      return await this.timezoneFromIp(conversation.customer.ipAddress);
    }

    // Try to parse from message content
    if (conversation.messages?.[0]?.content) {
      return this.timezoneFromContent(conversation.messages[0].content);
    }

    return null;
  }

  private async timezoneFromIp(ip: string): Promise<string | null> {
    // In production, use a geolocation service
    // For demo, return null
    return null;
  }

  private timezoneFromContent(content: string): string | null {
    // Simple pattern matching for timezone mentions
    const patterns = [
      /\b(EST|PST|CST|MST|GMT)\b/i,
      /UTC([+-]\d{1,2})/i
    ];

    for (const pattern of patterns) {
      const match = content.match(pattern);
      if (match) return match[1];
    }

    return null;
  }
}
```

### Step 5: Test Your Plugin

Create `src/index.test.ts`:

```typescript
import TimezonePlugin from './index';

describe('TimezonePlugin', () => {
  let plugin: TimezonePlugin;
  let mockContext: any;

  beforeEach(() => {
    plugin = new TimezonePlugin({ enabled: true });

    mockContext = {
      events: {
        on: jest.fn()
      },
      logger: {
        info: jest.fn()
      },
      services: {
        conversations: {
          addTag: jest.fn()
        }
      }
    };
  });

  it('should register event listener on init', async () => {
    await plugin.init(mockContext);

    expect(mockContext.events.on).toHaveBeenCalledWith(
      'conversation.created',
      expect.any(Function)
    );
  });

  it('should tag conversation with detected timezone', async () => {
    const conversation = {
      id: 'conv-123',
      customer: {
        timezone: 'America/New_York'
      }
    };

    await plugin.init(mockContext);

    const handler = mockContext.events.on.mock.calls[0][1];
    await handler({ conversation, context: mockContext });

    expect(mockContext.services.conversations.addTag).toHaveBeenCalledWith(
      'conv-123',
      'timezone:America/New_York'
    );
  });
});
```

### Step 6: Build the Plugin

Compile your TypeScript:

```bash
npm run build
```

### Step 7: Register the Plugin

Add to your Hay configuration (`hay.config.ts`):

```typescript
export default {
  plugins: [
    '@hay/plugin-timezone-tagger'
  ]
};
```

### Step 8: Run and Test

Start Hay in development mode:

```bash
npm run dev
```

Create a test conversation and verify:
1. The plugin initializes without errors
2. New conversations get tagged with timezone
3. Logs show plugin activity

## Next Steps

Now that you've built your first plugin, explore:

- **[Plugin API Reference](/docs/technical/plugins/api-reference/)** - Full API documentation
- **[Channel Architecture](/docs/technical/plugins/channel-architecture/)** - Understand the channel system
- **[Quick Reference](/docs/technical/plugins/quick-reference/)** - Handy reference guide

### Common Patterns

#### Adding Configuration UI

Plugins can register settings in the UI:

```typescript
async init(context: PluginContext): Promise<void> {
  context.ui.registerSettings({
    section: 'Timezone Tagger',
    fields: [
      {
        key: 'enabled',
        type: 'boolean',
        label: 'Enable automatic tagging',
        default: true
      },
      {
        key: 'tagPrefix',
        type: 'string',
        label: 'Tag prefix',
        default: 'timezone:'
      }
    ]
  });
}
```

#### Adding API Endpoints

Expose custom endpoints:

```typescript
async init(context: PluginContext): Promise<void> {
  context.api.registerRoute({
    method: 'GET',
    path: '/timezones',
    handler: async (req, res) => {
      const timezones = await this.getAllTimezones();
      res.json(timezones);
    }
  });
}
```

## Troubleshooting

### Plugin Not Loading

Check:
1. Plugin is listed in `hay.config.ts`
2. `package.json` has `"hay": { "plugin": true }`
3. Build completed without errors
4. No TypeScript compilation errors

### Events Not Firing

Verify:
1. Event name is spelled correctly
2. Event listener registered in `init()`
3. Plugin initialization completed successfully
4. Check logs for errors

Need help? Join our [Discord community](https://discord.gg/hay) or check the [API documentation](/docs/technical/plugins/api-reference/).
