# Backend development

Owns service configuration and the optional LiteLLM adapter. Host integration belongs in [Hou-lm](https://github.com/Byter3/Hou-lm) and [Ae-lm](https://github.com/Byter3/Ae-lm).

## Checks and packaging

Python 3.11, from the repository root:

```sh
python -m unittest discover -s tests -q
python scripts/package_gateway.py
```

Tests use synthetic fixtures; no live model or deployment. Missing LiteLLM, POSIX support or the optional colocated Hou-lm parser produces skips. Record them with results. The allowlisted source archive is written to ignored `dist/`; CI builds it after tests pass.

## Changes

Keep Compose, adapter logic, compatibility fingerprints and deployments independently reviewable. Preserve complete tool arguments/output, sanitized diagnostics and explicit package/Docker allowlists. Validate dependency changes with the [gateway workflow](gateway/README.md); retain [stack identity and rollback](docs/migration.md).

Before publishing, review the diff and archive for credentials, project data, histories and logs. Report tested dependency versions and actual checks; offline results do not establish live-provider compatibility. CI does not deploy services or publish releases.

[MIT](LICENSE).
