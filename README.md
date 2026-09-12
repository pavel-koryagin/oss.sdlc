I'm sharing here some of my SDLC playbooks.

## Index: When to Use and Which Playbook

- [`ai.evals/`](ai.evals/) - AI evaluation standards.
- [`how-to-work.production/`](how-to-work.production/) - production development stage. Deliver secure and reliable software.
- [`how-to-work.prototyping/`](how-to-work.prototyping/) - vibe prototyping stage. Fast and dirty, saving humans time.
  - [`single-task.e2e.md`](how-to-work.prototyping/single-task.e2e.md) — When requesting a single end-to-end task, describe the expected outcomes and add this file.
- [`principles/`](principles/) - platform-agnostic SDLC/coding/design principles.
- [`webapp.nextjs/`](webapp.nextjs/) - web app standards. Next.js-based stack.
- [`preferences.cursor.md`](preferences.cursor.md) — Add when using Cursor.

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
