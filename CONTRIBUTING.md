# Contributing to the studio assistant backend

This repository owns shared server setup and the optional LiteLLM stream adapter. Host UIs, project tools, command review, client credentials and local history belong in [Hou-lm](https://github.com/Byter3/Hou-lm) or [Ae-lm](https://github.com/Byter3/Ae-lm).

Read [migration and provenance](docs/migration.md) before changing deployment paths, image names or service configuration. Repository organization and deployment changes must remain separate.

## Offline checks

Use Python 3.11, from the repository root:

```sh
python -m unittest discover -s tests -q
python scripts/package_gateway.py
```

These checks use synthetic events, temporary files and subprocess fixtures. They do not request a live model, access artist project data or deploy a server. The builder writes only the allowlisted adapter source archive into `dist/`.

Tests requiring LiteLLM skip when it is not installed. POSIX-specific process/entrypoint tests skip on Windows. One retained optional integration test checks Hou-lm's parser only when its source is colocated; it skips in this standalone backend repository and is not a client dependency. Portable CI coverage does not establish actual provider compatibility.

To validate the adapter against a supported LiteLLM environment, use an isolated dependency environment and the [gateway workflow](gateway/README.md). The adapter supports only verified package versions/source fingerprints. Do not bypass those checks to make an unsupported upgrade appear green. The image build runs offline checks with the existing base image's Python and dependencies; a build is still separate from selecting that image for a running service.

## Change scope

- Keep Compose, adapter behavior, compatibility fingerprints and deployment procedures independently reviewable.
- Preserve complete tool arguments and assistant output; never replace a model request with a hardcoded artist-task recipe.
- Keep startup diagnostics bounded and sanitized. Do not print raw credentials, prompts or provider responses in errors.
- Keep the Docker context and source archive on explicit file allowlists.
- Update compatibility tests alongside adapter changes and record skipped environments.
- Retain the current deployment's stack name, volumes, credentials and rollback image when an independently authorized deployment is performed.
- Application plug-ins must remain independently installable. Do not add host imports, a scene-routing service, a cross-client execution endpoint or a shared client dependency here.

## Publishing

Review the exact staged files, commit metadata and archive entries before publication. Use a noreply author identity if desired. Never add populated `.env`, database/token-volume backups, private histories, keys or local diagnostic captures.

CI uses read-only repository permissions and uploads a source bundle after offline checks pass. It does not publish a release or deploy a service. Release notes must state which dependency-specific and live-route checks actually ran. Do not claim that a successful Houdini interaction validates every backend image or an unimplemented After Effects client.

Contributions are under the [MIT license](LICENSE).
