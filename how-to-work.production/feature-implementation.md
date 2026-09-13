# Feature Implementation Process

Wire up models, server actions, and controllers for a designed feature. Assumes views already exist with mock props.

**Input**: Existing `docs/specs/<Feature Name>.md` with UX and Views sections

## Steps

### Step 1: Plan Models & Logic

Append the Models and Server Actions sections to the spec file. Follow the principles from [[webapp.nextjs/code-guidelines.data|data guidelines]]

**Process**:
1. Review view props — what data do they require?
2. Design Zod schemas for entities
3. Design DTOs for forms and actions
4. Plan server actions needed
5. Create test samples

**Validation**: Types align with view prop requirements.

This is the template of what to append:

```markdown
## 3. Models

### Entities

- [ ] `<EntityName>` — Description of fields

### DTOs

- [ ] `<Name>Dto` — Form/action input

### Samples

- [ ] `<entity>Sample` — What we need to reflect our cases (study and reuse existing samples as much as possible)

## 4. Server Actions

- [ ] `<actionName>Action` — Description

## 5. Client Controllers

- [ ] `<pageName>Controller` — Description
```

If no model changes are required — reduce the part.

### Step 2: Implement Models

Create the persistence layer and update views. Follow [[webapp.nextjs/code-guidelines.data|data guidelines]]

**Process**:
1. Create Drizzle schemas
2. Generate and run migrations
3. Update view props to use real types (replace mock types)
4. Update Storybook samples to use typed samples

**Validation**: Views still render correctly with typed props.

### Step 3: Implement Logic

Wire everything together with controllers and server actions.

**Process**:
1. Create/update server actions per [[webapp.nextjs/code-guidelines.server-actions|server action guidelines]]
2. Create/update client controllers per [[webapp.nextjs/code-guidelines.components|component guidelines]]
3. Write E2E tests per [[webapp.nextjs/code-guidelines.tests|test guidelines]]

**Validation**: Feature works end-to-end. Tests pass.

## Tools and Principles

Intermediate validation helpers:
- `npm run lint:fix`
- `npm run local-ci`

Do intermediate commit after each step and even sub-step from this process. Do not push.
