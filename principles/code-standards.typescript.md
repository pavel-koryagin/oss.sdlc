# TypeScript Code Standards

## Use lodash-es

Use `lodash-es` (or `lodash`). Import the helpers you need.

If a built-in function matches a lodash operation, prefer the built-in. If lodash offers better semantics, prefer lodash.

Good:

```ts
Array.isArray(value);
```

Bad:

```ts
import { isArray } from 'lodash-es';

isArray(value);
```

Reason: they are identical, built-in wins.

Good:

```ts
import { isString } from 'lodash-es';

isString(value);
```

Tolerable:

```ts
typeof value === 'string';
```

Reason: "is string" sounds semantic, but the `typeof` version is popular and so is also recognizable by humans.

Good:

```ts
import { union } from 'lodash-es';

union(a, b);
```

Bad:

```ts
[...(new Set([...Object.keys(a), ...Object.keys(b)]))];
```

Reason: `union()` is semantic, its inline implementation is cryptic.
