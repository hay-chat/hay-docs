---
layout: docs.njk
title: Testing
description: E2E testing guide with Playwright
section: technical
navGroup: Contributing
navOrder: 5
---

# E2E Testing Guide

## Overview

This project uses Playwright for end-to-end testing with automatic authentication. Tests leverage storage state to authenticate once and reuse credentials across all tests, making them faster and more reliable.

## Quick Start

```bash
# Run all E2E tests
npx playwright test

# Run specific test file
npx playwright test tests/plugin-health-check.spec.ts

# Run with UI mode (interactive)
npx playwright test --ui

# Run specific browser
npx playwright test --project=chromium
npx playwright test --project=webkit

# Run in headed mode (see browser)
npx playwright test --headed

# Debug tests
npx playwright test --debug
```

## Authentication System

### Automated Tests (Storage State)

Tests automatically authenticate using saved auth state. No manual login required.

**How it works:**
1. **Global setup** creates a test user before tests run
2. Auth state saved to `playwright/.auth/user.json`
3. All tests load this auth state automatically
4. Tests can navigate directly to protected pages

**Test user credentials:**
- **Email**: `hay-e2e-YYYYMMDD@test.com` (date-based, e.g., `hay-e2e-20251220@test.com`)
- **Password**: `E2eTest@123456`
- **Role**: Owner
- **Organization**: `E2E Test Org YYYYMMDD`

### Manual Browser Access (URL Token Auth)

For debugging with Playwright MCP or manual testing, prefer `navigateWithAuth()` over
extracting tokens from the storage state file by hand:

1. **Use the login helper** in a test or Playwright MCP script:
   ```typescript
   import { navigateWithAuth } from "./tests/helpers/login";

   await navigateWithAuth(page, "/plugins");
   ```

   `navigateWithAuth()` (in `tests/helpers/login.ts`) logs in as the test user via the
   `auth.login` API to get a fresh `accessToken`/`refreshToken`/`expiresIn`, then navigates
   with those as URL params. This avoids the storage state file entirely, so there's no
   risk of using a stale or expired token.

2. Browser validates token and logs you in automatically

**Security notes:**
- Only works in development (`NODE_ENV !== 'production'`)
- Token removed from URL after validation
- Tokens expire after 15 minutes (JWT default)

**Note on the storage state file:** `playwright/.auth/user.json` stores the token inside a
JSON-stringified `pinia:auth` localStorage entry, and the expiry field there is `expiresAt`
(an absolute epoch-ms timestamp), not `expiresIn` (a relative seconds value). If you need to
navigate manually with the `?accessToken=&refreshToken=&expiresIn=` URL params, call the
`POST /v1/auth.login` endpoint directly (the same thing `navigateWithAuth()` does) to get a
fresh `expiresIn` value instead of trying to derive it from the storage state file.

## Writing Tests

### Basic Test Structure

```typescript
import { test, expect } from "@playwright/test";

test("My test", async ({ page }) => {
  // Navigate directly - auth already loaded from storage state
  await page.goto("/plugins");

  // Your test assertions
  await expect(page.locator("h1")).toContainText("Plugins");
});
```

### Best Practices

1. **Use relative URLs** (baseURL configured):
   ```typescript
   await page.goto("/plugins");  // ✅ Good
   await page.goto("http://localhost:3000/plugins");  // ❌ Verbose
   ```

2. **No manual login** - Auth state handles this automatically

3. **Wait for network idle** when page has dynamic content:
   ```typescript
   await page.goto("/plugins");
   await page.waitForLoadState("networkidle");
   ```

4. **Use semantic selectors** for resilient tests:
   ```typescript
   page.getByRole("button", { name: "Save" })  // ✅ Good
   page.locator("button.btn-primary")  // ❌ Fragile
   ```

5. **Set appropriate timeouts**:
   ```typescript
   test.setTimeout(30000);  // 30 seconds for slow operations
   ```

## Test Organization

### Test Files

The following is a non-exhaustive list of spec files:

- `tests/plugin-health-check.spec.ts` - Plugin connection status tests
- `tests/instructions-editor.spec.ts` - Instructions editor functionality
- `tests/instructions-editor-paste-and-slash.spec.ts` - Editor paste/slash tests
- `tests/webchat-plugin.spec.ts` - Webchat plugin tests
- `tests/zendesk-plugin.spec.ts` - Zendesk plugin tutorial tests
- `tests/atlassian-plugin.spec.ts` - Atlassian plugin tests
- `tests/email-plugin.spec.ts` - Email plugin tests
- `tests/email-plugin-installation.spec.ts` - Email plugin installation tests
- `tests/webchat-consent-strict.spec.ts` - Webchat consent (strict mode) tests
- `tests/hubspot-oauth-flow.spec.ts` - HubSpot OAuth flow tests
- `tests/plugin-health-check-and-settings.spec.ts` - Plugin health check and settings tests
- `tests/url-token-auth.spec.ts` - URL token authentication tests

### Test Helpers

- `tests/helpers/auth.ts` - Authentication utilities
  - `getTestUserEmail()` - Generate test user email
  - `getTestOrgName()` - Generate test org name
  - `cleanupTestUsers()` - Delete old test users
  - `createTestUser()` - Create test user + org + tokens
  - `generateAuthState()` - Create Playwright storage state
- `tests/helpers/login.ts` - Navigation utilities
  - `navigateWithAuth(page, path)` - Navigate to a path with URL token auth injected

### Global Setup

- `tests/global-setup.ts` - Runs before all tests
  - Initializes database connection
  - Cleans up old test users (pattern: `hay-e2e-%@test.com`)
  - Creates fresh test user + organization
  - Generates JWT tokens
  - Saves storage state to `playwright/.auth/user.json`

## Test Data Management

### Test User Pattern

Test users follow the pattern: `hay-e2e-YYYYMMDD@test.com`

**Automatic cleanup:**
- All users matching `hay-e2e-%@test.com` are deleted before each test run (plugin_instances deleted first, then users, then organizations separately)
- Ensures clean state for each test run

### Manual Cleanup

If needed, you can manually cleanup test users:

```typescript
import { cleanupTestUsers } from "./tests/helpers/auth";
import { AppDataSource } from "./server/database/data-source";

await AppDataSource.initialize();
await cleanupTestUsers();
await AppDataSource.destroy();
```

## Configuration

### Playwright Config

`playwright.config.ts` configuration:

```typescript
{
  globalSetup: "./tests/global-setup.js",  // Runs once before all tests
  baseURL: "http://localhost:3000",        // Default URL for page.goto()
  storageState: "playwright/.auth/user.json",  // Auth state file
  webServer: [
    // Auto-starts server and dashboard if not running
    { command: "cd server && npm run dev", url: "http://localhost:3001" },
    { command: "cd dashboard && PORT=3000 npm run dev", url: "http://localhost:3000" }
  ]
}
```

### TypeScript Config

`tests/tsconfig.json` extends root config with:
- CommonJS modules for Playwright compatibility
- Playwright type definitions
- Path aliases for server/dashboard imports

## Debugging

### View Test Execution

```bash
# Interactive UI mode
npx playwright test --ui

# Headed mode (see browser)
npx playwright test --headed

# Debug mode (step through)
npx playwright test --debug
```

### View Test Reports

```bash
# Run tests
npx playwright test

# Open HTML report
npx playwright show-report
```

### View Traces

Traces are automatically captured on first retry:

```bash
npx playwright show-trace trace.zip
```

### Console Logs

Tests log browser console output:

```typescript
page.on("console", (msg) => console.log("Browser:", msg.text()));
```

### Screenshots

Capture screenshots for debugging:

```typescript
await page.screenshot({ path: "debug.png", fullPage: true });
```

## Performance Benefits

### Before (Manual Login)
```typescript
await page.goto("/login");
await page.fill('input[type="email"]', "test@test.com");
await page.fill('input[type="password"]', "password");
await page.click('button[type="submit"]');
await page.waitForTimeout(2000);  // ~2-3 seconds
await page.goto("/plugins");
```

### After (Storage State)
```typescript
await page.goto("/plugins");  // Direct navigation, ~0 seconds for auth
```

**Performance gain**: ~2-3 seconds saved per test

## Troubleshooting

### "Storage state not found"

**Problem**: `playwright/.auth/user.json` doesn't exist

**Solution**: Global setup (`tests/global-setup.ts`) runs automatically on every invocation
of `npx playwright test` — there's no separate `@setup` tag or command to trigger it on its
own. Just re-run the test command and it will regenerate the file:
```bash
npx playwright test
```

### "Authentication failed"

**Problem**: Token expired or invalid

**Solution**: Re-run tests (global setup creates fresh tokens)
```bash
npx playwright test
```

### "Database connection failed"

**Problem**: PostgreSQL not running or `.env` misconfigured

**Solution**:
1. Start PostgreSQL: `brew services start postgresql@16`
2. Check `.env` has correct database credentials
3. Verify database exists: `psql -l`

### "Browser not found"

**Problem**: Playwright browsers not installed

**Solution**:
```bash
npx playwright install
```

### Tests fail after auth changes

**Problem**: Storage state structure outdated

**Solution**:
1. Update `generateAuthState()` in `tests/helpers/auth.ts`
2. Match new Pinia store structure
3. Re-run global setup

### URL token auth not working

**Problem**: Middleware not recognizing token

**Checklist**:
- Running in development (`NODE_ENV !== 'production'`)
- Token is valid (check expiry)
- All three parameters passed correctly in URL: `?accessToken=TOKEN&refreshToken=REFRESH_TOKEN&expiresIn=SECONDS`
- Check browser console for auth middleware logs

## CI/CD Integration

### GitHub Actions Example

> **Note:** This is a simplified illustration. For the actual workflow this project runs,
> see [`.github/workflows/playwright.yml`](../../../.github/workflows/playwright.yml).
> Key differences from the simplified example below: the real workflow is triggered by
> `workflow_dispatch` only (manual run from the Actions tab — it is **not** a merge gate
> and does not run on push/PR), it also spins up a Redis service and runs database
> migrations before the tests, and it configures the database via `DB_HOST`/`DB_PORT`/
> `DB_USERNAME`/`DB_PASSWORD`/`DB_NAME` env vars rather than a single `DATABASE_URL`.

```yaml
name: E2E Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: pgvector/pgvector:pg16
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 'lts/*'
          cache: 'npm'

      - name: Install dependencies
        run: npm install

      - name: Install Playwright browsers
        run: npx playwright install --with-deps

      - name: Run E2E tests
        run: npx playwright test
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/hay_test

      - name: Upload test results
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: playwright-report
          path: playwright-report/
```

## Security Considerations

### Production Safety

- URL token auth **disabled** in production (`NODE_ENV !== 'production'`)
- Tokens expire after 15 minutes (JWT config)
- Storage state files in `.gitignore` (prevents committing tokens)
- Test users have distinctive pattern for easy identification

### Test User Isolation

- Test users are owners of their own organizations
- Each test run gets a fresh user/org (clean state)
- Old test data automatically cleaned up
- No impact on production data

## Additional Resources

- [Playwright Documentation](https://playwright.dev/)
- [Playwright Best Practices](https://playwright.dev/docs/best-practices)
- [Debugging Tests](https://playwright.dev/docs/debug)
- [Test Reporters](https://playwright.dev/docs/test-reporters)

## Support

For issues or questions:
1. Check this documentation
2. Review `playwright.config.ts`
3. Check test examples in `/tests` directory
4. Open an issue on GitHub
