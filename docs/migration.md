# Repository extraction and existing deployments

The server example and optional adapter were extracted from [Hou-lm commit a86e87a6f8b7c42a8c6b17b73de4fabcf4768c2f](https://github.com/Byter3/Hou-lm/tree/a86e87a6f8b7c42a8c6b17b73de4fabcf4768c2f) on 22 October 2026. The MIT attribution is retained.

## What moved

| Area | New owner |
| --- | --- |
| Compose, empty environment template and server setup guide | This repository |
| Optional LiteLLM adapter, Dockerfile, launcher, build/check scripts and source packager | This repository |
| Adapter, probe, bootstrap and entrypoint tests | This repository |
| Houdini client, saved login, HIP sidecars, model-delivery diagnostics and installable ZIP | [Hou-lm](https://github.com/Byter3/Hou-lm) |
| Planned After Effects client and its project history | [Ae-lm](https://github.com/Byter3/Ae-lm) |

Compose, adapter source, executable scripts and migrated tests were copied unchanged. Documentation and repository scaffolding were adapted for their new location. This extraction changes ownership and development layout, not the service protocol or a running deployment.

Historical names such as `houlm-litellm:1.103.2-streamfix2`, `houlm-litellm-base:...`, `/opt/houlm`, `houlm-gateway-1.103.2-streamfix2.tar.gz` and the startup marker are intentionally retained. Renaming those during extraction would unnecessarily change the established build and rollback procedure.

## Existing studio installation

**Do not create a new stack or run `docker compose up` from this new checkout merely because the code moved.** Existing containers can keep running with their current configuration. Clients keep their current service URL and user credentials.

A new checkout directory name can change Compose's default project name, which can create a separate set of prefixed volumes rather than use the existing accounts and database. Preserve the exact existing Compose project/Portainer stack name and volume mappings whenever a later deployment change is explicitly intended. Do not guess the project name, recreate the stack or delete volumes to make paths match this repository.

For a separately planned update:

1. Record the existing stack/project identity, resolved volumes, image IDs and protected configuration using the studio's deployment process.
2. Back up persistent data and the secrets required to restore it.
3. Compare the intended source change with the currently deployed stack. Keep service names, environment values, container paths and volume mappings unless the update specifically requires a validated migration.
4. Follow the [server guide](server-setup.md) or [adapter guide](../gateway/README.md) for that change and its rollback. A source checkout alone is not a deployment instruction.

The old Hou-lm tags remain historical references. No backend move requires reinstalling or rebuilding the published Houdini client ZIP. Ae-lm remains a development scaffold and does not claim validated service or host integration yet.

## Validation boundaries

Offline tests run synthetic events and subprocess fixtures. Optional LiteLLM tests require their supported dependency environment; native POSIX entrypoint tests require their platform. No result from these tests establishes the contents or health of a live studio VM.

The confirmed [Houdini runtime baseline](https://github.com/Byter3/Hou-lm/blob/working-2026-10-15/docs/working-baseline.md) did not verify the exact deployed gateway image. Keep provider/version compatibility and live-route checks separate from repository extraction.
