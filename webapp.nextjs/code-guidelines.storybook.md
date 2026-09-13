# Storybook Patterns

## View Stories

**Location**: `src/app/<path/to/screen>/<Name>View.stories.tsx`
**Guidelines**:
- Strip `View` from the component title
- For pages: use `wrapIframe` option and `Pages` category
- For components: drop `wrapIframe` option, use `Components` category, drop Resolutions story

*Example*
```typescript
import { actions } from '@storybook/addon-actions';
import type { Meta, StoryObj } from '@storybook/react';

import { StorybookPageSlot, StorybookResolutionsShowcase } from '@/.storybook/StorybookSlot';
import { aliceUserSample, bobUserSample } from '@/tests/samples/users';
import { MainLayoutView } from '@/components/layouts/MainLayoutView';
import { ProfileView } from './ProfileView';

const meta = {
  title: 'Pages/Profile',
} satisfies Meta<typeof ProfileView>;

export default meta;
type Story = StoryObj<typeof meta>;

const defaultArgs: React.ComponentProps<typeof ProfileView> = {
  user: aliceUserSample,

  ...actions('onSomething'),
};

export const Profile: Story = {
  render: () => (
    <>
      <StorybookPageSlot title="Default State">
        <MainLayoutView currentPath="/profile">
          <ProfileView
            {...defaultArgs}
          />
        </MainLayoutView>
      </StorybookPageSlot>

      <StorybookPageSlot title="Different User">
        <MainLayoutView currentPath="/profile">
          <ProfileView
            {...defaultArgs}
            user={bobUserSample}
          />
        </MainLayoutView>
      </StorybookPageSlot>
    </>
  ),
};

export const Resolutions: Story = {
  render: () => (
    <StorybookResolutionsShowcase>
      <MainLayoutView currentPath="/profile">
        <ProfileView
          {...defaultArgs}
        />
      </MainLayoutView>
    </StorybookResolutionsShowcase>
  ),
};
```

## Layout Stories

**Location**: `src/components/layouts/<LayoutName>.stories.tsx`

*Example*
```typescript
import type { Meta, StoryObj } from '@storybook/react';

import { StorybookPageSlot, StorybookResolutionsShowcase } from '@/.storybook/StorybookSlot';
import { MainLayoutView } from './MainLayoutView';

const meta = {
  title: 'Layouts/Main',
} satisfies Meta<typeof MainLayoutView>;

export default meta;
type Story = StoryObj<typeof meta>;

export const Main: Story = {
  render: () => (
    <StorybookPageSlot title="Normal">
      <MainLayoutView currentPath="/home">
        The page content
      </MainLayoutView>
    </StorybookPageSlot>

    ... // Navigation states go here
  ),
};

export const Resolutions: Story = {
  render: () => (
    <StorybookResolutionsShowcase>
      <MainLayoutView currentPath="/home">
        The page content
      </MainLayoutView>
    </StorybookResolutionsShowcase>
  ),
};
```

## Journey Stories

**Location**: `tests/journeys/<Name>.stories.tsx`

**Guidelines**:
- Naming:
  - One file per journey
  - Main journey: `tests/journeys/Main.stories.tsx`
  - Other journeys: descriptive names like `tests/journeys/Subscription.stories.tsx`
- Use existing View components where available
- For screens without views yet, create simple placeholder mockups with plain HTML/text
- Each story represents a complete user journey
- Number the steps in slot titles
- Use `wrapIframe` for all journey slots

*Example `tests/journeys/1_main.stories.tsx`*
```typescript
import { actions } from '@storybook/addon-actions';
import type { Meta, StoryObj } from '@storybook/react';

import { StorybookPageSlot } from '@/.storybook/StorybookSlot';
import { MainLayoutView } from '@/components/layouts/MainLayoutView';
import { wordsSample } from '@/tests/samples/words';

import { CaptureModalView } from '@/app/words/CaptureModalView';
import { DashboardView } from '@/app/words/DashboardView';

const meta = {
  title: 'Journeys/Main',
} satisfies Meta;

export default meta;
type Story = StoryObj<typeof meta>;

const captureActions = actions('onSelectionChange', 'onContextChange', 'onSave', 'onCancel');
const dashboardActions = actions('onDeleteWord', 'onExport');
const layoutActions = actions('onSignOut', 'onSignIn', 'onLogoClick', 'onNavigateToDashboard', 'onNavigateToSettings');

export const Main: Story = {
  render: () => (
    <>
      <StorybookPageSlot title="1. Web Page with Unknown Word">
        ... // Some plain text
      </StorybookPageSlot>

      <StorybookPageSlot title="2. Capture Modal (Plugin)">
        <CaptureModalView
          selection="ubiquitous"
          context="Coffee shops have become ubiquitous in most major cities around the world."
          {...captureActions}
        />
      </StorybookPageSlot>

      <StorybookPageSlot title="3. Word Dashboard (Web App)">
        <MainLayoutView
          currentPath="/dashboard"
          userEmail="alice@example.com"
          {...layoutActions}
        >
          <DashboardView
            words={wordsSample}
            {...dashboardActions}
          />
        </MainLayoutView>
      </StorybookPageSlot>
    </>
  ),
};
```
