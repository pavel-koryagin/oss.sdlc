I'm sharing here some of my SDLC playbooks.

## Index: When to Use and Which Playbook

- `how-to-work.production/` - production development stage. Deliver secure and reliable software.
  - [[how-to-work.production/feature-design|feature-design]] — Design before implementation. Output: specs and Storybook views.
  - [[how-to-work.production/feature-implementation|feature-implementation]] — Implement the feature. Input: feature-design.md.
- `how-to-work.prototyping/` - vibe prototyping stage. Fast and dirty, saving humans time.
  - [[how-to-work.prototyping/single-task.e2e|single-task.e2e]] — When requesting a single end-to-end task, describe the expected outcomes and add this file.
- `principles/` - platform-agnostic SDLC/coding/design principles.
- `webapp.nextjs/` - web app standards. Next.js-based stack.
  - `code-guidelines.*` - code style to adhere to.
    - [[webapp.nextjs/code-guidelines.data|code-guidelines.data]] — Zod schemas, Drizzle database schemas, DTOs, and test samples.
    - [[webapp.nextjs/code-guidelines.components|code-guidelines.components]] — Layouts, views, forms, and client controllers.
    - [[webapp.nextjs/code-guidelines.server-actions|code-guidelines.server-actions]].
    - [[webapp.nextjs/code-guidelines.errors|code-guidelines.errors]] — Error-handling.
    - [[webapp.nextjs/code-guidelines.tests|code-guidelines.tests]].
    - [[webapp.nextjs/code-guidelines.storybook|code-guidelines.storybook]] — Storybook stories.
    - [[webapp.nextjs/code-guidelines.android|code-guidelines.android]] — Android TWA wrapper and Google Play publishing.
  - [[webapp.nextjs/design-guidelines|design-guidelines]] — Visual design principles and libraries.
  - [[webapp.nextjs/icons|icons]] — App icon design and generation.
- [[preferences.cursor|preferences.cursor]] — Add when using Cursor.
- [[preferences.sandbox|preferences.sandbox]] — Add when using sandbox environments.
- `snippets/` — Helpful snippets for situations not covered by the playbooks.
  - [[snippets/minimal-sandbox-resources|minimal-sandbox-resources]] — Provision minimal shared sandbox resources.

## Example of Use

Assuming:

- Using Cursor in the cloud
- A repo with a multi-tenant web app you are working on is selected
- The app is in the prototyping stage
- The docs in the repo and the agent environment match our standards

```
Implement a Super Admin user role and a UI section to manage tenants. Design the architecture and UI yourself.

Follow these playbooks:
https://raw.githubusercontent.com/pavel-koryagin/oss.sdlc/v1/how-to-work.prototyping/single-task.e2e.md
https://raw.githubusercontent.com/pavel-koryagin/oss.sdlc/v1/preferences.cursor.md
```

## Licence

This project is licensed under the [[LICENSE|MIT License]], and everyone is welcome to use, modify, and share it.
