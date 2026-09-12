## Environment

Use OP_SERVICE_ACCOUNT_TOKEN and OP_ENVIRONMENT_ID to read the 1password env. Other vars are there.

Avoid reading secrets but feel free to read other vars to your context. They contain your knowledge.

Feel free to use these var values including secrets in your sandbox deployments when needed.

Feel free to edit everything in the accounts, accessible with those secrets. They are your sandboxes.

## GCP

- To sandbox GCP project, deploy with GCP Infrastructure Manager. If repo's normal deployment is not built on GCP Infrastructure Manager, do not use it with sandbox.
- In sandbox, create DBs in a single shared instance. Do not create new DB instances.
    - Postgres: `postgres-db`
    - Other (Mongo, Redis, etc.) - find an instance. If absent, create a generic one. Use as multi-tenant.

## AgentMail

- New inbox per sandbox deployment, format `{something}-{YYMMDDHHMM}@agentmail.to`. Configure webhook if needed.
- When the account limit is exhausted, delete the oldest inbox.

## OpenRouter

The key is use-only.
