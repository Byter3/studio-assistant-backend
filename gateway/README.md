# Optional LiteLLM streaming adapter

For missing tool arguments or answer text on affected **ChatGPT provider** routes. `streamfix2` recovers final Responses snapshots without duplicating calls or rewriting commands. A working route does not need it.

## Build

Requires an existing **LiteLLM 1.103.2** container with the verified production entrypoint and `/app` layout. Version/fingerprint checks reject incompatible builds; do not bypass them.

From the backend checkout:

```sh
python scripts/package_gateway.py
```

Transfer `dist/houlm-gateway-1.103.2-streamfix2.tar.gz` to the Docker host:

```sh
tar -xzf houlm-gateway-1.103.2-streamfix2.tar.gz
cd houlm-gateway-1.103.2-streamfix2
sh gateway/build-image.sh YOUR_LITELLM_CONTAINER
```

The builder uses the container's exact base image and prints:

- New image: `houlm-litellm:1.103.2-streamfix2`
- Rollback image: `houlm-litellm-base:...`

It does not restart the container.

## Deploy and verify

In the **existing** stack, change only `litellm.image` to the new tag. Disable re-pull for this local image; retain variables and volumes. Look for:

```text
Hou-lm stream repair revision: streamfix2 (tool arguments and answer text).
```

Verify a tool request **and its final answer** through [Hou-lm](https://github.com/Byter3/Hou-lm) or [Ae-lm](https://github.com/Byter3/Ae-lm). [Synthetic delivery checks](https://github.com/Byter3/Hou-lm/blob/main/docs/tool-stream-troubleshooting.md#run-synthetic-checks) are also available; they contact the model but run no scene commands.

## Rollback

Restore `litellm.image` to the saved rollback tag and update with re-pull disabled. For future LiteLLM versions, test stock behavior before carrying this adapter forward.

[Backend setup](https://github.com/Byter3/studio-assistant-backend/blob/main/docs/server-setup.md) · [Existing-stack updates](https://github.com/Byter3/studio-assistant-backend/blob/main/docs/migration.md)
