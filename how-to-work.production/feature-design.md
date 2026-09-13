# Feature Design Process

Design-first approach to feature development. Define UX and build view components with Storybook stories before implementation.

## Steps

### Step 1: Design UX

Define the user experience before any code.

**Deliverables**:
- User story with clear benefit
- Step-by-step flow description
- Edge cases and error states

**Validation**: Can you walk through the entire flow verbally?

Create `docs/specs/<Feature Name>.md` with the following structure:

```markdown
# <Feature Name>

## 1. UX Design

### User Story

As a [user type], I want to [action] so that [benefit].

### User Journey

- [emoji] **[Screen Name]:** [Description of user action and screen content.]  

- [emoji] **[Screen Name]:** [Description of user action and screen content.]

- ...

### Edge Cases

- What happens when...
- Error state for...

## 2. Views

### Screens

- [ ] `<ScreenName>View` — Description
- [ ] `<ScreenName>View` — Description

### Components

- [ ] `<ComponentName>` — Reusable component for...

### Stories

- [ ] Feature journey story in `tests/journeys/` only if the feature introduces a new multi-screen user flow
- [ ] Individual view stories

### Views Checklist

- [ ] Create view components with mock props
- [ ] Create Storybook stories
- [ ] Verify all states in Storybook
```

- Adjust the definitions according to the task. If we only update views, not introduce them — mention the ones we update.
- If the scope is complex enough, worth several user journeys - define them, use nested header.
- Use these standard screen names when applicable:
	- Landing
		- Do not clarify much. Marketing is out of scope for this process.
		- A proto-landing should be in our app template. Do not modify it.
	- Sign-in/Sign-up Page
		- This assumes all the auth-related pages including password reset, do not clarify as it is a standard component.
	- Settings
		- I.e. user profile editing page.
	- Billing
		- For everything related to payments. Do not describe separate screens as they should be template.
- For each screen, provide a complete description of its body layout and all visible components. Use a list to detail every element a user can see or interact with.
	- Every screen from these description are going to be implemented by UI designer. Provide all the information they need.
	- Do not include third-party screens from user journey.
	- It is phone screen only, unless another screen sizes are explicitly targeted.
	- Specify which app layout the screen uses (or "None" for popups/modals).
- The Main journey should be comprehensive.
	- Start with a new not registered user.
	- Pass through all the steps to give them value.
	- Do interactions with the main feature.
	- When paid features are defined (usually not in the beginning), include a paywall and payment in the journey.
- Other journeys should be narrowed - start where their ambit begins.
	- Consider updating the Main journey, if the feature is of the core value.

### Step 2: Design Views

Build the UI layer with hardcoded mock data. Views are pure presentation — no business logic yet.

**Process**:
1. Create view components per [[webapp.nextjs/code-guidelines.components|component guidelines]]
2. Use placeholder props (strings, mock objects)
3. Create Storybook stories per [[webapp.nextjs/code-guidelines.storybook|Storybook guidelines]]
4. Cover all visual states: empty, loading, error, success, edge cases

- Tick the items in the feature doc as you go.
- Use existing `tests/samples/**` files, do not define new at this stage.

**Validation**: All screens visible and interactive in Storybook.

## Tools and Principles

Do not run lint or tests at this stage. They are for the next process. Only ensure that Storybook builds before handing over the results.

Do intermediate commit after each step and even sub-step from this process. Do not push.
