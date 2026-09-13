# Tests

## Integration Tests

### Key Principles

1. **Mock at the highest level possible** - Mock `getSession` directly instead of mocking DB sessions and headers
2. **Use db directly** - No wrapper functions for simple DB operations in tests
3. **Use `toStrictEqual`** - Prefer strict equality over partial matching
4. **Make dates deterministic** - Use `timekeeper` to freeze time
5. **Use semantic constants** - Define date constants with meaningful names

### AAA Pattern

- Always use comments to denote sections parts.
- Multi-step scenarios are also allowed, but the steps should be additionally named.
- Skip the empty parts.

*Example, simple*

```ts
// Arrange
...

// Act
const result = await subject(...);

// Assert
expect(result).toStringEqual(...);
```

*Example, scenario*

```ts
// Arrange
...

// Act - Subscribe
...

// Assert - Subscribe
...

// Act - Unsubscribe
...

// Assert - Unsubscribe
```

*Example, asserting exception in AAA*

```ts
// Act
let error: Error | undefined;
try {
  await subject();
} catch (e: any) {
  error = e;
}

// Assert
expect(e).to...
```

### DB Seed and Mocking

Use samples from `tests/samples/`. Add new samples there too.

#### Authentication Mocking

Mock `getSession` from `@/libs/auth` - this cuts off both DB and headers access:

```typescript
vi.mock('@/libs/auth', () => ({
  getSession: vi.fn(),
}));

const { getSession } = await import('@/libs/auth');
```

Use session samples from `tests/samples/dtos/auth.ts`.

### Time Management

Use `timekeeper` for deterministic dates:

```typescript
import timekeeper from 'timekeeper';

const today = '2024-01-01T00:00:00Z';
const inOneMonth = '2024-02-01T00:00:00Z';

afterAll(() => {
  timekeeper.reset();
});

it('test case', async () => {
  timekeeper.freeze(today);
  // ... test code
});
```

### Database Operations

Use drizzle directly - no wrappers needed:

```typescript
// Insert
await db.insert(dbUser).values(aliceUser);

// Select
const finalUsers = await db.select({
  billingPlanId: dbUser.billingPlanId,
  billingPaidTill: dbUser.billingPaidTill,
}).from(dbUser);
```

Assume connection to a dedicated database. Reset in tests with DB:

```ts
beforeEach(async () => {
  await devTools.resetAppStateToEmpty();
});
```

Assert the whole table, not only the manipulated records.

```ts
const finalUsers = await db.select({ ... }).from(dbUser);
expect(finalUsers).toStrictEqual([{
  // exact expected values
}]);
```

### Test Structure

```typescript
import timekeeper from 'timekeeper';

import { db } from '@/libs/DB';
import { dbUser } from '@/models/db/user';
import { aliceSession } from '@/tests/samples/dtos/auth';
import { aliceUser } from '@/tests/samples/User';
import { devTools } from '@/tests/utils/devTools';

vi.mock('@/libs/auth', () => ({
  getSession: vi.fn(),
}));

const { getSession } = await import('@/libs/auth');

const today = '2024-01-01T00:00:00Z';
const inOneMonth = '2024-02-01T00:00:00Z';

beforeEach(async () => {
  await devTools.resetAppStateToEmpty();
});

afterAll(() => {
  timekeeper.reset();
});

describe('Feature E2E', () => {
  it('scenario description', async () => {
    // Arrange
    timekeeper.freeze(today);
    await db.insert(dbUser).values(aliceUser);
    vi.mocked(getSession).mockResolvedValue(aliceSession);

    // Act
    const result = await someAction({ ... });

    // Assert
    expect(result?.data).toStrictEqual({
      paidTill: new Date(inOneMonth),
    });

    const finalUsers = await db.select({ ... }).from(dbUser);
    expect(finalUsers).toStrictEqual([{
      // exact expected values
    }]);
  });
});
```

### DevTools

Use `devTools` from `tests/utils/devTools.ts`
