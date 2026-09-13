# Error Handling

**Pattern**: Throw errors directly, let the framework handle them.

## Core Principles

1. **Use `throw new Error()`** for all error types
   - TODO: Import custom Error types from the old boilerplate. Not done yet.
1. **Never wrap with try/catch** unless specific error-handling logic was **explicitly requested** for that function or endpoint
2. **Never re-throw with different message** - this loses the original error context

## Anti-patterns

❌ **Never catch and re-throw with a different message:**

```typescript
// WRONG - loses original error context
try {
  await someOperation();
} catch (e) {
  throw new Error('Another error message');
}
```

❌ **Never add try/catch without specific handling logic:**

```typescript
// WRONG - unnecessary wrapper
try {
  await someOperation();
} catch (e) {
  throw e; // pointless
}
```

## Correct Usage

✅ **Let errors propagate naturally:**

```typescript
// CORRECT - no try/catch needed
await someOperation();
```

✅ **Only catch when you have specific handling logic:**

```typescript
// CORRECT - has actual handling logic
try {
  await externalApiCall();
} catch (e) {
  logger.error('External API failed', e);
  await fallbackOperation();
}
```
