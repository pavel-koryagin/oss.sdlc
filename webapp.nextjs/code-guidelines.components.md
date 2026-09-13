# Component Patterns

## Layout Components

**Location**: `src/components/layouts/<LayoutNameView>.tsx`
**Pattern**: Pure presentation component

*Example `src/components/layouts/MainLayoutView.tsx`*
```typescript
import type { ReactNode } from 'react';

export const MainLayoutView = (props: {
  children: ReactNode;
  currentPath?: string;
}) => (
  ...
);
```

## View Components

**Location**: `src/app/<path/to/screen>/<Name>View.tsx`
**Pattern**: Pure presentation component that receives all data as props

**Rules**:
View-controller-stories triade
- "Pure presentation" means UI logic only, not business logic. Hooks for presentation (i18n, context, forms, etc.) are expected.
- Avoid unless asked explicitly: useState, useEffect. Put them into the controller and get in the view as props.
- Every view prop should be mandatory. Mock them all in storybook, do not skip any.
- Type domain data props with models or DTOs from `src/models/`; keep shared data shapes out of view files. View prop types may add presentation-only primitives, callbacks, and state.
i18n
- Views MUST use `useTranslations` for i18n - this is part of our definition of a pure presentation component.
- Views can use hooks like `useTranslations` - we don't need extreme pureness.
- For formatting (prices, dates, numbers), use `useI18nFormatting` from `@/utils/formatting`. Read that file at session start to discover available utilities.
- Translation strings should NOT be passed as props. Each component must call `useTranslations` internally. Exception: Email templates cannot use i18n hooks yet.
- Legal document pages should be static, though still structured with a view. Their texts, titles, and modification timestamps should not use i18n and be always inline static in English.
Bundling
- Views by default have no 'use client' directive.
- Views with forms need 'use client' directive.

*Example*
```typescript
import { useTranslations } from 'next-intl';
import type { User } from '@/types/User';
import { useI18nFormatting } from '@/utils/formatting';

export type ProfileViewProps = {
  user: User;
  subscriptionPrice: number;
}

export const ProfileView = ({
  user
}: ProfileViewProps) => {
  const t = useTranslations('Profile');
  const { formatPrice } = useI18nFormatting();

  return (
    <div>
      <h1>{t('title')}</h1>
      <p>{user.name}, {formatPrice(subscriptionPrice)}</p>
    </div>
  );
};
```

When no props, the type should still be explicitly defined.

*Example*
```
// ...
export type TermsViewProps = Record<string, never>;

export const TermsView = (_props: TermsViewProps) => {
// ...
```

## Forms

For forms, use React Hook Form with Zod validation and shadcn/ui Form components.

**View Component Pattern**:
- The View manages its own form state using `useForm`
- Props should be **non-optional** (use `string | null` instead of `string?`)
- The form scope should be **narrow** - only wrap actual form fields and submit button
- Non-form buttons (e.g., OAuth, navigation) should be outside the `<form>` element
- Views with forms need 'use client' directive.
- For navigation use `Link` from `@/libs/i18nNavigation`, not the event props.

*Example `SignUpView.tsx`*
```typescript
'use client';

import { zodResolver } from '@hookform/resolvers/zod';
import { useTranslations } from 'next-intl';
import { useForm } from 'react-hook-form';
import { Form, FormControl, FormField, FormItem, FormLabel, FormMessage } from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { Button } from '@/components/ui/button';
import { Link } from '@/libs/i18nNavigation';
import { signUpDtoSchema, type SignUpDto } from '@/models/dto/SignUpDto';

export type SignUpViewProps = {
  onSubmit: (data: SignUpDto) => void;
  onGoogleSignUp: () => void;
  isLoading: boolean;
  error: string | null;  // Non-optional, use null instead of undefined
};

export const SignUpView = ({
  onSubmit,
  onGoogleSignUp,
  isLoading,
  error,
}: SignUpViewProps) => {
  const t = useTranslations('SignUp');
  const form = useForm<SignUpDto>({
    resolver: zodResolver(signUpDtoSchema),
  });

  return (
    <div>
      {/* Error display - outside form */}
      {error && <div className="error">{error}</div>}

      {/* OAuth button - outside form */}
      <Button onClick={onGoogleSignUp}>{t('google_button')}</Button>

      {/* Form scope - narrow, only wraps form fields */}
      <Form {...form}>
        <form onSubmit={form.handleSubmit(onSubmit)}>
          <FormField
            control={form.control}
            name="email"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('email_label')}</FormLabel>
                <FormControl>
                  <Input {...field} />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />

          {/* Other fields... */}

          <Button type="submit" disabled={isLoading}>
            {isLoading ? t('saving_button') : t('save_button')}  
          </Button>
        </form>
      </Form>

      {/* Navigation links - outside form */}
      <Link href="/sign-in">Sign in</Link>
    </div>
  );
};
```

### Forms with no redirect

If a form remans after the action is processed, show indication like this:

```tsx
<div className="flex items-center gap-3">
    <Button
      type="submit"
      disabled={isLoading}
    >
      {isLoading ? t('saving_button') : t('save_button')}
    </Button>
    {personalInfoSuccess && (
      <span className="animate-alert-fade-out text-sm text-green-600 dark:text-green-400">
        {t('saved_message')}
      </span>
    )}
  </div>
</form>
```

## Controller Components

**Pattern**: Created after the View component to manage state and logic

- Do not create static controllers - include those trivial views from the page components.
- Create controllers with state, useEffect, forms, data loading, etc. They need 'use client' directive.

*Example `SignUpForm.tsx`*
```typescript
'use client';

import { useTranslations } from 'next-intl';
import { useState } from 'react';
import { useRouter } from '@/libs/i18nNavigation';
import type { SignUpDto } from '@/models/dto/SignUpDto';
import { SignUpView } from './SignUpView';

export const SignUpForm = () => {
  const t = useTranslations('SignUp');
  const router = useRouter();
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const handleSubmit = async (data: SignUpDto) => {
    setError(null);
    setIsLoading(true);

    try {
      // API call here
      router.push('/dashboard');
    } catch (err) {
      setError(err instanceof Error ? err.message : t('error_unknown'));
      setIsLoading(false);
    }
  };

  return (
    <SignUpView
      onSubmit={handleSubmit}
      onGoogleSignUp={handleGoogleSignUp}
      isLoading={isLoading}
      error={error}
    />
  );
};
```
