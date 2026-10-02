## Environment

Use OP_SERVICE_ACCOUNT_TOKEN and OP_ENVIRONMENT_ID to read the 1password env. Other vars are there.

Avoid reading secrets but feel free to read other vars to your context. They contain your knowledge.

Feel free to use these var values including secrets in your sandbox deployments when needed.

Feel free to edit everything in the accounts, accessible with those secrets. They are your sandboxes.

If you deployed anything to the sandbox, include the public URLs into your testing report.

## Scoping

Use a YYMMDDHHMM suffix in resource ids of your deployments to logically connect the deployments in different providers.

Generate it from system UTC time when you need it first, and then keep using the same value during the session.

## GCP

- To sandbox GCP project, deploy with GCP Infrastructure Manager. If repo's normal deployment is not built on GCP Infrastructure Manager, do not use it with sandbox.
- In sandbox, create DBs in a single shared instance. Do not create new DB instances.
    - Postgres: `postgres-db`
    - Other (Mongo, Redis, etc.) - find an instance. If absent, create a generic one. Use as multi-tenant.
- Prefer default service domains.
    - In case you need custom domains, use zone from GCP_DNS_SANDBOX_ZONE_ID. Generate a per-session random 8-letter suffix and ensure it is added to each of the subdomains you create.

## AgentMail

- New inbox per sandbox deployment, format `{something}-{YYMMDDHHMM}@agentmail.to`. Configure webhook if needed.
- When the account limit is exhausted, delete the oldest inbox.

## OpenRouter

The key is use-only.
