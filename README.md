# Studio assistant backend

Shared server setup and an optional LiteLLM compatibility adapter for studio assistant clients, including [Hou-lm](https://github.com/Byter3/Hou-lm) and the [Ae-lm development project](https://github.com/Byter3/Ae-lm).

This repository owns the Open WebUI / LiteLLM / PostgreSQL / optional Ollama example, server documentation, and the optional gateway adapter with its build tools and offline checks. Each client owns its application interface, local tools, conversation history and installable package. There is no shared executable client core here.

```text
Houdini client -------------------\
                                  > Open WebUI -> LiteLLM -> configured models
After Effects client (planned) ---/

Tool requests execute locally in the originating application.
```

**Existing deployments need no change because this repository was separated.** Keep the current stack/project name, images, secrets, volumes and client URL. Cloning into a differently named directory must not silently create a new Compose project or new database volumes. Read [migration and provenance](docs/migration.md) before adopting these files on an existing host.

## Contents

| Path | Purpose |
| --- | --- |
| [compose.yaml](compose.yaml) | Optional central service example; floating image tags, HTTP endpoints and an NVIDIA-backed Ollama service |
| [.env.example](.env.example) | Empty required-secret template; populate a private copy only |
| [Server setup](docs/server-setup.md) | New deployments, model routing, user keys and persistent data |
| [gateway/](gateway/README.md) | Optional version-checked LiteLLM ChatGPT stream adapter |
| `scripts/package_gateway.py` | Explicit-file source bundle for that adapter |
| `scripts/diagnose_litellm_stream.py` | Synthetic offline converter probe; no model request |
| `tests/` | Adapter, converter, bootstrap and entrypoint fixtures |

Use an existing compatible studio service where one is already working. The example stack is not a pinned production baseline. The optional adapter targets particular LiteLLM source versions and is unnecessary for routes that already deliver complete tool calls and answers.

## Validate or package locally

With Python 3.11, from this repository:

```sh
python -m unittest discover -s tests -q
python scripts/package_gateway.py
```

The standard-library suite skips optional LiteLLM integration tests when that dependency is unavailable and skips POSIX checks on Windows. See [Contributing](CONTRIBUTING.md) for the distinction between offline, dependency-specific and deployment validation.

The builder produces `dist/houlm-gateway-1.103.2-streamfix2.tar.gz`. Its legacy artifact/image names are retained to preserve the existing build and rollback workflow. It is an adapter source archive, not an installer for the entire server stack or either application client. Generated artifacts stay out of Git.

The CI workflow runs portable offline tests on Windows and Linux and builds that source archive. It does not access a studio deployment, use provider credentials or deploy containers.

## Security and licensing

Secrets, private projects, local histories and production configuration do not belong in this repository. Review [security boundaries](SECURITY.md), especially HTTPS/network access, provider credentials and persistent-volume backups.

[MIT](LICENSE), copyright 2026 Byter3. Open WebUI, LiteLLM, Ollama, PostgreSQL, host applications and provider services retain their own licenses and terms.
