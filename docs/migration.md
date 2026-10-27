# Update an existing stack

Keep the existing **Compose project/Portainer stack name, volume mappings, credentials and LiteLLM salt key**. A directory-name change can otherwise attach new, empty volumes.

1. Back up the stack definition, persistent volumes and secrets; record current image IDs.
2. Apply the intended configuration/image change to the existing stack.
3. Verify Open WebUI login, model access and a client tool-call/result/answer exchange.
4. Retain the previous images and backups until accepted.

Changing `POSTGRES_PASSWORD` in Compose does not rotate credentials in an initialized database. Do not remove volumes to update an installation.

Client updates require no server rebuild. Existing `houlm-litellm:*` image names remain valid.

[Server setup](server-setup.md) · [Adapter build/rollback](../gateway/README.md) · [Hou-lm](https://github.com/Byter3/Hou-lm) · [Ae-lm](https://github.com/Byter3/Ae-lm)
