# Updating an existing installation

A new checkout or client update does not require recreating the server stack. Keep using the existing Open WebUI URL and artist credentials.

## Preserve the deployment identity

Docker Compose normally derives a project name from the checkout directory. Changing that name can create new prefixed volumes instead of attaching the existing accounts and databases. Keep the existing Compose project or Portainer stack name and its exact volume mappings.

Before an update:

1. Record the current stack definition, project name, image IDs and volume mappings.
2. Back up persistent data and the protected configuration needed to restore it, including the LiteLLM salt key.
3. Compare the proposed configuration with the running deployment. Preserve service names, credentials and storage paths unless the update requires a deliberate migration.
4. Apply the change to the existing stack and verify the client route. Retain the previous images and backups until the update is accepted.

Do not delete volumes or create a replacement stack simply to match this repository's directory name. Changing the PostgreSQL password in Compose alone does not rotate a password in an initialized database.

For service configuration, use [Server setup](server-setup.md). The optional adapter has its own [build, update and rollback instructions](../gateway/README.md).

## Older Hou-lm deployments

The backend files were originally included in Hou-lm. They now live here; client packages remain in their respective repositories. Existing image names such as `houlm-litellm:1.103.2-streamfix2`, rollback tags and `/opt/houlm` paths still apply. The move does not require reinstalling a client or renaming a running service.

The original server sources remain available in [Hou-lm's extraction reference](https://github.com/Byter3/Hou-lm/tree/a86e87a6f8b7c42a8c6b17b73de4fabcf4768c2f).
