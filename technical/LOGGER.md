# Logging Guide

Hay uses [Pino](https://getpino.io/) for structured, PII-redacting logging across the server.

## Quick Start

```typescript
import { createLogger } from "@server/lib/logger";

const logger = createLogger("my-module");

logger.info("Operation completed");
logger.info({ userId: "123", duration: 45 }, "Operation completed with context");
logger.error({ err: error }, "Operation failed");
logger.warn("Deprecation notice");
logger.debug("Detailed trace info");
```

**Pino calling convention** — object first, message second:

```typescript
// Structured data + message
logger.info({ orderId, status }, "Order processed");

// Error logging (use `err` key for proper serialization)
logger.error({ err: error, orderId }, "Failed to process order");

// Simple message only
logger.info("Server started");
```

## Log Levels

| Level   | Use for                                        |
| ------- | ---------------------------------------------- |
| `error` | Failures requiring attention                   |
| `warn`  | Degraded behavior, recoverable issues          |
| `info`  | Important state changes, startup, success      |
| `debug` | Diagnostic detail, request tracing, dev output |

Configure via `LOG_LEVEL` environment variable. Defaults to `debug` unless `LOG_LEVEL` is explicitly set.

## PII Redaction

All logs are automatically redacted at two layers:

### Layer 1: Field-based redaction (structured data)

Sensitive fields are replaced with `[REDACTED]` before serialization. The list below is a representative subset; see `server/lib/logger/redaction.ts` for the full source of truth.

- **Credentials**: `password`, `token`, `accessToken`, `refreshToken`, `apiKey`, `secret`, `clientSecret`
- **Personal data**: `email`, `phone`, `phoneNumber`, `ssn`, `creditCard`, `bankAccount`
- **Headers**: `headers.authorization`, `headers.cookie`
- **Nested**: All of the above at three depths of nesting (`field`, `*.field`, `*.*.field`)

### Layer 2: Regex-based string redaction (log messages)

Sensitive patterns embedded in freeform text are automatically caught, including email addresses, phone numbers, JWTs, Bearer/Basic auth tokens, IPv4 addresses, credit card numbers, SSNs, and common API key prefixes (e.g., `sk-`, `pk_live_`):

```typescript
logger.info("User john@example.com signed up");
// Output: "User [EMAIL_REDACTED] signed up"

logger.info("Call +1-555-123-4567 for support");
// Output: "Call [PHONE_REDACTED] for support"
```

## Output Format

- **Development**: Human-readable colored output via `pino-pretty`
- **Production**: ndjson (newline-delimited JSON) to stdout
- **Test**: JSON to stdout (no pretty-printing)

## Configuration

| Variable        | Default       | Description                          |
| --------------- | ------------- | ------------------------------------ |
| `LOG_LEVEL`     | `debug`       | Log level (always defaults to `debug` unless explicitly set) |
| `DEBUG_MODULES` | `*`           | Module filter for debug output         |

## ESLint Enforcement

`console.*` is banned in server code (`no-console: "error"`). Exceptions:

- Test files (`*.test.ts`, `*.spec.ts`, `tests/setup.ts`)
- Migration files (`database/migrations/`)
- CLI scripts (`scripts/`, `run-migration.ts`)

## Log Retention

Production logs must be retained for **no more than 30 days** per GDPR requirements.
This is configured at the infrastructure level:

- **AWS CloudWatch**: Set log group retention to 30 days
- **Docker/K8s**: Configure log driver `max-size`/`max-file` or use a log aggregation service with 30-day retention
- **Self-hosted**: Use `logrotate` with 30-day maximum

## Files

- `server/lib/logger/index.ts` — Root logger and `createLogger()` factory
- `server/lib/logger/redaction.ts` — PII redaction patterns and string sanitizer
- `server/tests/lib/logger.test.ts` — Redaction unit tests
