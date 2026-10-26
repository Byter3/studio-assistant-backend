# Contributing to the studio assistant backend

This repository owns server setup and the optional LiteLLM streaming adapter. Application UIs, project tools, command review and local history belong in [Hou-lm](https://github.com/Byter3/Hou-lm) or [Ae-lm](https://github.com/Byter3/Ae-lm).

## Offline checks and packaging

Use Python 3.11 from the repository root:

```sh
python -m unittest discover -s tests -q
python scripts/package_gateway.py
```

Tests use synthetic events, temporary files and subprocess fixtures. They do not contact a live model or deploy a server. The builder produces the allowlisted adapter source archive in `dist/`; keep generated archives out of Git.

LiteLLM integration tests skip when that dependency is unavailable; POSIX checks skip on Windows. The optional check against Hou-lm's parser also skips when its source is not colocated. No sibling checkout is required for this backend or its clients.

Validate adapter compatibility in an isolated supported LiteLLM environment using the [gateway workflow](gateway/README.md). Do not bypass package-version or source-fingerprint checks. Record relevant passes and skips in the pull request; offline checks do not establish live provider or deployment compatibility.

## Change scope

- Keep Compose, adapter behavior, compatibility fingerprints and deployment changes independently reviewable.
- Preserve complete tool arguments and assistant output. Do not add task-specific command recipes.
- Keep startup errors bounded and sanitized; never print credentials, prompts or raw provider responses.
- Keep the Docker context and source archive on explicit file allowlists.
- Update regression checks when changing adapter behavior.
- Preserve an existing deployment's [project identity, volumes and rollback options](docs/migration.md).
- Keep application plug-ins independently installable. Do not add host imports, cross-client execution endpoints or runtime sibling-checkout dependencies.

A repository update or image build does not itself deploy a service. Keep real model requests and changes to running containers separate from offline checks.

## Release review

Review the intended diff, commit metadata and archive entries. Exclude populated environments, credentials, database/token backups, private projects, histories and diagnostic captures. Use synthetic examples in tests and public reports.

CI has read-only repository permissions and uploads the adapter source bundle after offline checks pass. It does not publish a release or deploy containers. State the supported dependency versions and the checks actually performed in release notes.

Contributions are under the [MIT license](LICENSE).
