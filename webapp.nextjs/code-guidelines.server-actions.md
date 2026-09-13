# Server Actions

Server actions use `next-safe-action` for type-safe server-side operations with automatic validation.

**Location**: `src/features/<feature>/<actionName>.action.ts` or `src/app/<path>/<actionName>.action.ts` (e.g. `createScan.action.ts`, `getUserProfile.action.ts`).
**Pattern**: Type-safe server actions with input/output validation
**Naming**: camelCase with `Action` suffix (e.g., `createScanAction`, `getUserProfileAction`). The `Action` suffix is important to distinguish server actions from regular functions.

## Minimal Example

File: `createItem.action.ts`

```typescript
'use server';

import { z } from 'zod';

import { authActionClient } from '@/libs/safe-action';
import { db } from '@/libs/DB';

/**
 * Create a new item
 */
export const createItemAction = authActionClient
  .inputSchema(
    z.object({
      name: z.string().min(1, 'Name is required'),
    }),
  )
  .outputSchema(
    z.object({
      id: z.string().uuid()
    })
  )
  .action(async ({ parsedInput, ctx }) => {
    const { user } = ctx;

    const itemId = crypto.randomUUID();

    await db.insert(itemSchema).values({
      id: itemId,
      userId: user.id,
      name: parsedInput.name,
    });

    return { id: itemId };
  });
```

## Architecture Policies

- **Use `authActionClient`** - For authenticated actions. Thoroughly choose between `authActionClient` and `actionClient` based on whether it is allowed to call this action from guests.
- **Void output** - When returning only the fact of success, use `.outputSchema(z.void())`
- **Throw errors directly** - Do not invent alternative ways of returning errors. All errors should be thrown per [error-handling guidelines](code-guidelines.errors.md)

## Security Best Practices

- **Always use `authActionClient`** unless the action is meant to be called by guests
- **Validate ownership in queries**
- **Output schemas** - Always define output schemas for type safety

## Better Auth Integration

To add custom fields to the user object, configure them in `src/libs/auth.ts`:

```typescript
export const auth = betterAuth({
  user: {
    additionalFields: {
      customField: {
        type: 'string',
        required: false,
        input: false, // Set to true if field should be settable during registration
      },
    },
  },
});
```

## Calling Server Actions

Use `decodeSafeActionResult` from `@/utils/safe-action` to handle action results. This function throws errors on validation or server errors, making error handling consistent.

```typescript
import { decodeSafeActionResult } from '@/utils/safe-action';

// ...

try {
  const result = decodeSafeActionResult(
    await createItemAction(data),
  );

  // Use the result
  doSomething(result.id);
  router.push(`/items/${result.id}`);
} catch (err) {
  setError(err instanceof Error ? err.message : t('error_unknown'));
  setIsLoading(false);
}
```

When the action completes successfully and you don't need the result still use `decodeSafeActionResult`:

```typescript
decodeSafeActionResult(
  await completeOnboardingAction(data),
);
```
