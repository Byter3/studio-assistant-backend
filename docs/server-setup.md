# Server setup

Use this guide to configure the shared model service for [Hou-lm](https://github.com/Byter3/Hou-lm) and [Ae-lm](https://github.com/Byter3/Ae-lm). A compatible existing Open WebUI installation is sufficient; the Docker stack below is optional.

```text
Application plug-in → Open WebUI → LiteLLM → configured local or cloud model
```

Tools run inside the originating application. The backend does not open project files or provide a separate workstation command server.

## Requirements

- A Linux Docker host with Compose, or Portainer managing Docker Standalone.
- NVIDIA Container Toolkit and GPU access for the example's Ollama service. Adapt that service for a cloud-only or CPU deployment.
- Persistent storage, backups and protected network access.
- A model supporting tool calls. Image references and viewport feedback also require image input.

The [Compose example](../compose.yaml) uses floating tags for Open WebUI, LiteLLM and Ollama, plus PostgreSQL `16`. Select and record tested image versions for your studio. It publishes HTTP ports **8088** and **4000** without configuring TLS or access rules; provide those for your environment.

For an existing stack, read [Updating an existing installation](migration.md) first. The steps below describe a new deployment.

## Deploy a new stack

Copy [.env.example](../.env.example) to a private `.env` beside `compose.yaml` on the Docker host. Supply a different strong value for each variable:

| Variable | Value |
| --- | --- |
| `LITELLM_MASTER_KEY` | `sk-` followed by a long random value; reserve this key for administration. |
| `LITELLM_SALT_KEY` | A long random value; retain it with database backups. |
| `LITELLM_DB_PASSWORD` | A long random value using letters and numbers, because the example embeds it in a connection URL. |

For example, run `openssl rand -hex 32` separately for each value. Never commit the populated `.env`.

From the repository root on the Docker host:

```sh
docker compose up -d
docker compose ps
```

In Portainer, create a stack from `compose.yaml`, enter the same three environment variables and deploy. Empty required values are rejected. Provider credentials are configured in LiteLLM after startup.

## Connect models and Open WebUI

1. Open `http://YOUR_SERVER:4000/ui` and complete the [LiteLLM admin setup](https://docs.litellm.ai/docs/proxy/ui).
2. Add the desired provider models and credentials. Give each model a public alias such as `studio-model`.
3. Create a [LiteLLM virtual key](https://docs.litellm.ai/docs/proxy/virtual_keys) restricted to the aliases artists should use.
4. Open `http://YOUR_SERVER:8088`. In Open WebUI's admin connection settings, add an **OpenAI-compatible / Chat Completions** connection using `http://litellm:4000/v1` and that virtual key. See the [Open WebUI connection guide](https://docs.openwebui.com/getting-started/quick-start/connect-a-provider/starting-with-openai-compatible/).
5. Send a short browser chat using the alias and confirm the request appears in LiteLLM.

The example leaves `OPENAI_API_KEY` empty so the restricted key can be supplied in the UI. Open WebUI's saved settings may take precedence over Compose defaults; verify its saved connection when updating a deployment. Native Ollama access and user direct connections are disabled in the example so requests use LiteLLM.

### Optional local model

List models in Ollama:

```sh
docker compose exec ollama ollama ls
```

To download one, replace `MODEL_TAG` with its actual tag:

```sh
docker compose exec ollama ollama pull MODEL_TAG
```

In Portainer, run `ollama ls` or `ollama pull MODEL_TAG` in the Ollama container console.

Register the model in LiteLLM with these values:

| Setting | Value |
| --- | --- |
| Upstream model | `ollama_chat/MODEL_TAG` |
| API base | `http://ollama:11434` |
| Public alias | For example, `studio-local` |
| API key | Empty for this example's internal Ollama service |

Use the exact tag from `ollama ls`. Check the model's capabilities and [LiteLLM's Ollama configuration](https://docs.litellm.ai/docs/providers/ollama); text replies alone do not establish tool or image support.

## Give artists access

Each artist uses the **Open WebUI base URL and their Open WebUI user API key**. Do not give clients the LiteLLM master key, routing key or upstream provider credentials. Enable user API keys through [Open WebUI's access settings](https://docs.openwebui.com/features/authentication-access/api-keys/) as needed.

Follow the client installation guide:

- [Hou-lm](https://github.com/Byter3/Hou-lm/blob/main/docs/package-install.md)
- [Ae-lm](https://github.com/Byter3/Ae-lm/blob/main/docs/install.md)

Test the complete tool-call/result/answer flow before making project changes.

In Hou-lm, enable **Scene** and send:

> Use scene tools to list the immediate children of /obj. Report their names and types. Do not modify anything or force a cook.

In Ae-lm, open a disposable composition and send:

> Inspect the active composition using tools. Report its name, dimensions, duration and layer count. Do not modify anything, render or start a preview.

Check that a completed tool receipt appears and the answer matches its result. Test image input separately with a small non-sensitive reference if artists will use visual feedback. Confirm each user's model access and persistence after a planned restart.

## Persistent data

| Volume | Container path | Contents |
| --- | --- | --- |
| `ollama_data` | `/root/.ollama` | Local models and state |
| `open-webui-data` | `/app/backend/data` | Open WebUI accounts, chats and configuration |
| `litellm_db_data` | `/var/lib/postgresql/data` | Gateway models, keys and configuration |
| `litellm_chatgpt_data` | `/var/lib/litellm/chatgpt` | Authentication files if the optional ChatGPT provider is used |

Volume names normally include the Compose project or Portainer stack prefix. Keep that prefix and the LiteLLM salt key when restoring data. PostgreSQL's initial credentials apply only to an empty data directory; use the database's password-rotation procedure for an existing database. See the [PostgreSQL image documentation](https://hub.docker.com/_/postgres).

Ollama and PostgreSQL are reachable only inside the example stack; neither has a published host port.

## Troubleshooting

First test the same model in Open WebUI, then inspect the client error and corresponding gateway request. A gateway success entry does not prove the client received a complete tool call and final answer.

- [Hou-lm response diagnostics](https://github.com/Byter3/Hou-lm/blob/main/docs/tool-stream-troubleshooting.md)
- [Ae-lm installation and troubleshooting](https://github.com/Byter3/Ae-lm/blob/main/docs/install.md)
- [Optional LiteLLM ChatGPT streaming adapter](../gateway/README.md), for affected verified versions only

A working route needs no gateway rebuild to install or update an application plug-in.
