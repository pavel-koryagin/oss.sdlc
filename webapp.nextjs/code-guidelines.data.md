# Data Models

The application uses two separate schema systems:

## Zod Validation Schemas (Application Layer)

Define isomorphic data models using Zod for validation and TypeScript type inference.

**Location**: `src/models/[Entity].ts`
**Naming**:
- Schema export: lowercase camelCase with `Schema` suffix (e.g., `userSchema`, `scanSchema`)
- Type export: PascalCase matching entity name (e.g., `User`, `Scan`)
- Shared primitive schemas live in `src/models/types.ts` (e.g. `apiDateSchema` / `ApiDate`)
**Purpose**: Validation, type inference, business logic, isomorphic use across frontend and backend

*Example `src/models/User.ts`*
```typescript
import { z } from 'zod';

export const userSchema = z.object({
  id: z.string().min(1),
  email: z.string().email('Please enter a valid email address'),
});

export type User = z.infer<typeof userSchema>;
```

## Drizzle Database Schemas (Persistence Layer)

Define database table structures using Drizzle ORM.

**Location**: `src/models/db/[feature].ts`
**Naming**:
- Export name: prefix with `db` (e.g., `dbUser`, `dbAuthSession`) or use descriptive name (e.g., `scanSchema`)
- Table name: lowercase snake_case in database (via `pgTable('table_name', ...)`)
**Re-export**: All schemas must be re-exported from `src/models/db/schema.ts`
**Purpose**: Database table definitions, migrations, ORM queries

*Example `src/models/db/user.ts`*
```typescript
import { boolean, pgTable, text, timestamp } from 'drizzle-orm/pg-core';

export const dbUser = pgTable('user', {
  id: text('id').primaryKey(),
  email: text('email').notNull().unique(),
});
```

*Example `src/models/db/schema.ts`* (Central re-export file)
```typescript
export { scanSchema } from './scan';
export { dbAuthSession, dbAuthAccount, dbAuthVerification } from './auth';
```

**Important Notes:**
- Zod schemas and Drizzle schemas are **separate** - they may have slight differences (e.g., `z.date()` vs `timestamp()`)
- Drizzle config should point to `./src/models/db/schema.ts`
- Generate migrations with `npm run db:generate` after modifying Drizzle schemas
- Use JSONB columns with `.$type<YourZodType>()` for complex nested structures

## DTOs

Create dedicated DTO (Data Transfer Object) schemas in `src/models/dto/` for form/action payloads, and for read projections shared among 2+ different pages.

**Location**: `src/models/dto/[Name]Dto.ts`
**Naming**:
- Schema export: camelCase with `DtoSchema` suffix (e.g., `signUpDtoSchema`, `onboardingDtoSchema`)
- Type export: PascalCase with `Dto` suffix (e.g., `SignUpDto`, `OnboardingDto`)

*Example `src/models/dto/SignUpDto.ts`*
```typescript
import { z } from 'zod';

export const signUpDtoSchema = z.object({
  email: z.string().email('Please enter a valid email address'),
  password: z.string().min(8, 'Password must be at least 8 characters'),
  firstName: z.string().min(1, 'First name is required'),
  lastName: z.string().min(1, 'Last name is required'),
});

export type SignUpDto = z.infer<typeof signUpDtoSchema>;
```

# Test Data Samples

**Location**: `tests/samples/`
**Typing**: Strongly typed using inferred types from Zod schemas
**Naming**:
- File name: PascalCase matching entity name (e.g., `User.ts`, `Scan.ts`)
- Sample exports: camelCase with descriptive prefix and `Sample` suffix (e.g., `aliceUserSample`, `processedScanSample`)
- Array exports: pluralized with `Sample` suffix (e.g., `usersSample`, `scansSample`)
- Every entity sample file must include its plural array export, even when it is not currently used.
- Keep entity fixtures in entity sample files; keep joined/read-projection fixtures in `tests/samples/dtos/`, typed by their DTOs.
- Samples may import only models/DTOs, global types, and other samples — never views or components.
- Shared testing-universe moments live in `tests/samples/moments.ts`. Name dates by semantics with the `TestDate` suffix (e.g. `todayTestDate`) and reuse them only when the semantics match.
- Inline scalar constants when used once; extract consts only when reused.

*Example `tests/samples/User.ts`*
```typescript
import type { User } from '@/models/User';

export const aliceUserSample: User = {
  id: 'alice',
  email: 'alice@example.com',
};

export const usersSample: User[] = [
  aliceUserSample,
];
```
