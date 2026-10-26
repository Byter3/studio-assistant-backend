# Optional LiteLLM streaming adapter

This adapter addresses missing tool arguments or assistant answer text in specific versions of LiteLLM's **ChatGPT provider**. It is unnecessary for other providers or a route that already returns complete tool calls and answers.

Revision `streamfix2` retains content supplied in final Responses snapshots when incremental deltas omit it. It preserves call identities, suppresses duplicates and rejects conflicting or incomplete streams. It does not rewrite model-generated commands.

For general setup, use the [server guide](https://github.com/Byter3/studio-assistant-backend/blob/main/docs/server-setup.md). Diagnose the failing client route before changing its gateway image.

## Compatibility

The supplied build workflow requires an existing **LiteLLM 1.103.2** container with the verified production entrypoint and `/app` layout. Package-version, source-fingerprint and synthetic checks reject unsupported installations.

The module also contains guarded support for the older 1.83.14 test environment, but the image-building workflow targets 1.103.2. Do not bypass its checks for a different version.

Importing the module does not activate it. The bundled `gateway/bin/litellm` launcher installs the adapter before the ordinary CLI and in supported spawned workers. Application plug-ins do not import this module.

## Build the adapter image

From this repository's root:

```sh
python scripts/package_gateway.py
```

This creates `dist/houlm-gateway-1.103.2-streamfix2.tar.gz`, containing the allowlisted adapter source, checks and license. It is source for a Docker image, not a complete stack installer.

Transfer the archive to your Docker host. In a host terminal:

```sh
tar -xzf houlm-gateway-1.103.2-streamfix2.tar.gz
cd houlm-gateway-1.103.2-streamfix2
sh gateway/build-image.sh YOUR_LITELLM_CONTAINER
```

Replace `YOUR_LITELLM_CONTAINER` with the actual running container name. The account needs Docker access.

The builder uses that container's exact local image ID without pulling a replacement base. Checks run with its Python and dependencies, with networking disabled. The result is `houlm-litellm:1.103.2-streamfix2`; the script also prints a `houlm-litellm-base:...` rollback tag. Save the rollback tag. Building does not restart or modify the running container.

## Apply it to the existing stack

Back up the current stack configuration, secrets and persistent volumes first. Preserve the stack/project name and volume mappings; see [Updating an existing installation](https://github.com/Byter3/studio-assistant-backend/blob/main/docs/migration.md).

In Portainer's **existing stack Editor**, change only the LiteLLM service's image:

```yaml
  litellm:
    image: houlm-litellm:1.103.2-streamfix2
```

Update with **Re-pull image disabled** because the image was built locally. Keep the current ports, environment, volumes, model aliases and Open WebUI connection.

Check startup logs for:

```text
Hou-lm stream repair revision: streamfix2 (tool arguments and answer text).
```

The marker identifies the launcher revision. Verify normal server startup and the full client route before accepting the update.

## Verify delivery

Test both a tool call and the final answer after its result. A plain text reply or a gateway success status alone does not cover that flow.

For Hou-lm, use its [synthetic response diagnostics](https://github.com/Byter3/Hou-lm/blob/main/docs/tool-stream-troubleshooting.md#run-synthetic-checks) from a Hou-lm checkout. These contact the model but execute no scene commands. Then enable **Scene** in a new panel chat and send:

> Use scene tools to list the immediate children of /obj. Report their names and types. Do not modify anything or force a cook.

For Ae-lm, open a disposable composition and send:

> Inspect the active composition using tools. Report its name, dimensions, duration and layer count. Do not modify anything, render or start a preview.

Check each client's completed tool receipt and the answer based on it. Test through the actual client and Open WebUI route used by artists.

## Roll back or upgrade

To roll back, set `litellm.image` to the saved `houlm-litellm-base:...` tag and update the same stack with re-pull disabled. Preserve volumes and secrets.

For a LiteLLM upgrade, first test the intended stock version. If it handles the affected stream patterns correctly, use it without this adapter. Otherwise review source compatibility before building a new image.
