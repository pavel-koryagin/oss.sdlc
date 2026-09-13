I'm sharing here some of my SDLC playbooks.

## Index: When to Use and Which Playbook

- `how-to-work.production/` - production development stage. Deliver secure and reliable software.
  - [feature-design](how-to-work.production/feature-design.md) — Design before implementation. Output: specs and Storybook views.
  - [feature-implementation](how-to-work.production/feature-implementation.md) — Implement the feature. Input: feature-design.md.
- `how-to-work.prototyping/` - vibe prototyping stage. Fast and dirty, saving humans time.
  - [single-task.e2e](how-to-work.prototyping/single-task.e2e.md) — When requesting a single end-to-end task, describe the expected outcomes and add this file.
- `principles/` - platform-agnostic SDLC/coding/design principles.
- `webapp.nextjs/` - web app standards. Next.js-based stack. See Index in [webapp.nextjs/README](webapp.nextjs/README.md).
- [preferences.cursor](preferences.cursor.md) — Add when using Cursor.
- [preferences.sandbox](preferences.sandbox.md) — Add when using sandbox environments.
- `snippets/` — Helpful snippets for situations not covered by the playbooks.
  - [minimal-sandbox-resources](snippets/minimal-sandbox-resources.md) — Provision minimal shared sandbox resources.

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

This project is licensed under the [MIT License](LICENSE), and everyone is welcome to use, modify, and share it.
