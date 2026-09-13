# Single-Task SDLC

## Expected-Result Fidelity

- Before starting, ensure all supplied materials are usable and inspectable. If they are not, report and halt.
- Match the definition of the expected result.
- Use accompanying materials only to resolve what is unclear in the expected result. Do not implement anything from them that is not reflected in the expected result.
- Do not add extras. Of the unspecified behavior or architecture, implement only what is absolutely mandatory to make the expected result work.
- Derive test data from the supplied materials so testing reproduces the input data if any. As a second priority prefer the DB seed and test fixtures that are alrady in the repo.

## Minimal Delivery

- Do nothing that is not mandatory for the minimal implementation.
- Make unspecified decisions yourself.
- Keep every technical decision minimal. Deliver cheaply, but working as requested.
- Keep deployment minimal. If the task requires to change the deployment but does not specify how - choose or increment the existing process minimally.

## Execution

- Chunk the work and use subagents for planning, implementation, and QA.
- After every subagent, compare the delivered result with the expected result.
- When a broken deliverable is identified, loop until it is repaired.

## QA

- Automated tests are not required. Add them only when they help get a working solution faster.
- Plan QA and leave the corresponding documentation.
- Perform QA to certify the deliverable.
- Before delivery, compare the result with the expected result and ensure no element of its definition or accompanying materials was missed.
- Minimize test deployments.
    - If no external integration is affected by the test scenario, test locally. Do not use the cloud sandbox.
    - If external integrations are affected, test local while you develop and test E2E in the sandbox before you deliver.
    - If the deployment process itself is affected by the task requirements, test in the sandbox.
    - When using the sandbox, skip the components not used in the scenarios you are going to test. E.g. if the scenario is auth, then you don't need an AgentMail inbox.

## Documentation

- Document all your decisions in the repo's `docs/`.
- Plan the documents you leave and their directory structure thoroghly.
- Inspect concistency of documents and code both within and to each other.

## Environment and Resources

- Every credential you have (cloud deployment, external services) is a sandbox account. Use them for final E2E testing when applicable. Do not be afraid to break something there.
- If the sanbox is missing some external service or some permission so you cannot test the result E2E - report it, do not circumvent.
