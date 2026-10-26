# Security

## Credentials and project data

- Only the empty `.env.example` belongs in the repository. Keep populated environments, provider keys, user tokens and volume backups private.
- Artists use their own Open WebUI keys. Use restricted LiteLLM virtual keys for the Open WebUI connection and reserve the master key for administration.
- The configured service and upstream providers receive prompts, bounded project observations, tool results and any images submitted by clients. Configure access and retention for studio data.
- Keep the LiteLLM salt key with protected database backups so stored credentials remain recoverable.

## Network and execution

The Compose example publishes plain HTTP ports 8088 and 4000 on host interfaces. It does not configure TLS, a reverse proxy or firewall rules. Set up protected access and select tested image versions for your environment; the example uses floating tags.

Tool execution stays inside the originating application. Model output can influence commands offered to artists, and Auto-run is not a sandbox. Use trusted connections and review permissions appropriate to the project. This backend provides no workstation listener or generic remote host-execution endpoint.

The optional adapter's version and source checks establish compatibility for the supported paths, not general security of LiteLLM or its dependencies.

## Build and update

Build from a trusted checkout and inspect source archives before sharing. Archive and Docker inputs use explicit allowlists. The source packager rejects leaf symlinks but does not reject linked parent directories; ensure the checkout and its parent directories are trusted.

Preserve deployment identity, credentials and persistent data when updating; see [Updating an existing installation](docs/migration.md). Back up before replacing running services.

## Report a vulnerability

Use private GitHub vulnerability reporting if enabled. Otherwise request a private reporting channel before sharing sensitive material. Public reports should contain synthetic reproductions and sanitized errors, never secrets, project histories or production configuration.
