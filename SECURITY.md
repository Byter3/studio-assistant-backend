# Security

- Keep populated environments, provider/user keys and volume backups private. Only the empty `.env.example` belongs in Git.
- Clients use Open WebUI user keys; Open WebUI uses restricted LiteLLM virtual keys. Reserve the master key for administration.
- Prompts, project observations, tool results and submitted images reach the configured model provider. Apply studio access and retention policies.
- Compose publishes plain HTTP ports 8088/4000 and uses floating image tags. Configure protected access and tested versions.
- Back up database volumes with the LiteLLM salt key. Preserve [deployment identity](docs/migration.md) during updates.

Commands execute in the application clients; this backend has no workstation execution endpoint. Build from a trusted checkout: package allowlists do not protect against altered source or linked parent directories.

Report vulnerabilities privately through GitHub, or request a private channel. Exclude secrets and production data from public reports.
