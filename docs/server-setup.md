# Backend setup

Shared service for [Hou-lm](https://github.com/Byter3/Hou-lm) and [Ae-lm](https://github.com/Byter3/Ae-lm). Use an existing compatible Open WebUI instance or deploy the [Compose stack](../compose.yaml) below.

## Deploy

Requires Docker Compose/Portainer. The Ollama service requests an NVIDIA GPU; adapt it for CPU or cloud-only use. Pin suitable image versions and provide protected access to the published HTTP ports: **8088** (Open WebUI), **4000** (LiteLLM).

Copy [.env.example](../.env.example) to `.env` and supply distinct random values:

| Variable | Requirement |
| --- | --- |
| `LITELLM_MASTER_KEY` | `sk-` prefix; administration only |
| `LITELLM_SALT_KEY` | Retain with database backups |
| `LITELLM_DB_PASSWORD` | Alphanumeric; embedded in the example's database URL |

```sh
docker compose up -d
docker compose ps
```

For Portainer, deploy `compose.yaml` with the same variables. Existing deployments: preserve their [stack identity and volumes](migration.md).

## Route models

1. Open `http://YOUR_SERVER:4000/ui`. Add provider models/credentials and public aliases in LiteLLM.
2. Create a virtual key restricted to those aliases.
3. Open `http://YOUR_SERVER:8088`. Add an OpenAI-compatible **Chat Completions** connection to `http://litellm:4000/v1` using that key.
4. Enable Open WebUI user API keys and grant artists their model access.

Open WebUI's stored connection settings can override Compose defaults. Clients use **Open WebUI user keys**, not provider or LiteLLM keys. Models need tool calling; visual workflows also need image input.

For local inference:

```sh
docker compose exec ollama ollama pull MODEL_TAG
docker compose exec ollama ollama ls
```

Register `ollama_chat/MODEL_TAG` in LiteLLM with API base `http://ollama:11434`, no API key, and a public alias such as `studio-local`.

## Connect clients

Give artists the Open WebUI base URL and their user API key:

- [Install Hou-lm](https://github.com/Byter3/Hou-lm/blob/main/docs/package-install.md)
- [Install Ae-lm](https://github.com/Byter3/Ae-lm/blob/main/docs/install.md)

Test a tool request and its final answer through the installed client:

> Hou-lm: List the immediate children of /obj using scene tools. Do not modify anything or force a cook.

> Ae-lm: Inspect the active composition using tools. Report its name and layer count. Do not modify anything or render.

## Storage and diagnostics

Back up `open-webui-data`, `litellm_db_data`, `litellm_chatgpt_data`, configuration and the salt key. `ollama_data` holds downloaded models. Volume names include the stack/project prefix.

For failures, compare the client's receipt/error with the corresponding Open WebUI/LiteLLM request. A successful gateway log does not prove complete tool delivery. See [Hou-lm diagnostics](https://github.com/Byter3/Hou-lm/blob/main/docs/tool-stream-troubleshooting.md), [Ae-lm troubleshooting](https://github.com/Byter3/Ae-lm/blob/main/docs/install.md#troubleshooting), or the [optional streaming adapter](../gateway/README.md).
