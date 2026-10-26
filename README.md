# Studio assistant backend

Shared model service setup for [Hou-lm for Houdini](https://github.com/Byter3/Hou-lm) and [Ae-lm for After Effects](https://github.com/Byter3/Ae-lm).

Artists connect their plug-in to Open WebUI with their own account. Open WebUI routes requests to the studio's configured models, optionally through LiteLLM. Scene and composition commands execute inside the artist's application.

```text
Hou-lm / Ae-lm → Open WebUI → LiteLLM → local or cloud models
```

## Set up the service

If your studio already has a compatible Open WebUI service, use it. Installing a client does not require deploying this stack.

For a new deployment, follow [Server setup](docs/server-setup.md). The included Docker Compose example provides:

- Open WebUI for artist accounts and model access.
- LiteLLM and PostgreSQL for model routing and stored gateway configuration.
- Ollama for local models, configured to use an NVIDIA GPU.

The example uses floating image tags and plain HTTP ports. Configure versions, protected access and backups for your studio before deployment. See [Security](SECURITY.md).

For an existing stack, follow [Updating an existing installation](docs/migration.md) to preserve its project name, volumes and credentials.

## Connect the clients

Install each plug-in using its own guide:

- [Hou-lm installation](https://github.com/Byter3/Hou-lm/blob/main/docs/package-install.md)
- [Ae-lm installation](https://github.com/Byter3/Ae-lm/blob/main/docs/install.md)

Supply the Open WebUI base URL and an Open WebUI user API key. Choose a model with tool-call support; reference images and viewport feedback also need image input. No backend checkout is needed on artist workstations.

## Optional streaming adapter

The [LiteLLM ChatGPT streaming adapter](gateway/README.md) addresses missing tool arguments or answer text on specific verified LiteLLM versions. Use it only for an affected route. It is not required for ordinary client installation.

To build its source archive:

```sh
python scripts/package_gateway.py
```

The output is `dist/houlm-gateway-1.103.2-streamfix2.tar.gz`. This is adapter source, not a server installer or client plug-in. The existing artifact and image names are retained for compatibility.

## Development

See [Contributing](CONTRIBUTING.md) for offline checks, packaging and release review. CI builds the source archive after its offline checks pass; it does not deploy containers.

## License

[MIT](LICENSE), copyright 2026 Byter3. The separately installed applications, server components and model services have their own licenses and terms.
