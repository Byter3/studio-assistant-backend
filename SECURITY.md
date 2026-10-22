# Backend security boundaries

This repository contains an optional server example and a narrowly version-checked gateway adapter. It does not certify a live deployment's authentication, network exposure, installed container binaries or multi-user isolation.

## Credentials and data

- `.env.example` is deliberately empty. Populated environment files, provider keys, LiteLLM administration keys, user tokens and volume backups are private deployment data.
- Artists connect with their own Open WebUI user keys. Use restricted LiteLLM virtual keys for the Open WebUI-to-LiteLLM connection and reserve master credentials for administration.
- Open WebUI and upstream providers can receive client prompts, bounded project observations and tool results. Configure retention and access controls for the studio's project data.
- Keep the LiteLLM salt key with protected database backups; retaining database files without the required secret is not a complete recovery plan.

## Network and execution

The Compose example publishes HTTP ports 8088 and 4000 on host interfaces. It does not set up TLS, a reverse proxy or firewall rules. Configure authenticated access and HTTPS or an appropriately protected transport before using it in a studio environment. Floating image tags require deployment-specific review and version recording.

Host tool execution remains inside the originating client application. An attacker who controls model output or can modify unencrypted response traffic may influence commands offered to the artist; client review and Auto-run are not a sandbox. This repository adds no workstation listener or generic remote host-execution endpoint.

The optional adapter rejects unsupported source versions and conflicting/incomplete streams. Its checks are compatibility guardrails, not a general security boundary for LiteLLM or its provider dependencies.

## Packaging and provenance

The adapter source archive and Docker context use explicit allowlists. Build from a trusted checkout and inspect archive contents before sharing. The inherited source builder rejects leaf symlinks but does not yet reject linked parent directories; parent-link/containment hardening is a separate pending improvement, not part of the unchanged extraction.

The legacy image and artifact names are kept for compatibility. Updating a repository checkout does not authorize changing running containers. See [migration](docs/migration.md) for preserved deployment identity.

## Reporting

Use private GitHub vulnerability reporting if enabled. Otherwise request a private reporting channel before sharing sensitive material. Public reports should use synthetic reproductions and sanitized counters, never secrets, full project histories, provider responses or production configuration.
