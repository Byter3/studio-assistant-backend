# Working on the studio assistant backend

- Read `docs/migration.md`, `docs/server-setup.md` and `gateway/README.md` before changes.
- This is the shared server repository. Host-specific client code belongs in Hou-lm or Ae-lm; do not create cross-client command execution or a shared client core here.
- Initial executable files and Compose were extracted unchanged from the published Hou-lm reference. Keep repository moves separate from runtime, compatibility or deployment changes.
- Preserve existing adapter/image names and source fingerprint checks unless an explicit, separately tested compatibility change requires an update.
- Use the offline commands in CONTRIBUTING.md. Real model requests, image builds on a VM and changes to running containers are separate operations, not prerequisites for documentation work.
- A repository clone or folder rename must never imply redeployment. Preserve an existing stack/project name, private variables and volume mappings before any separately authorized deployment.
- Keep credentials, populated environments, token/database volumes, private project data, local backups and diagnostics out of commits and archives.
- Do not read or copy a local `.env` when preparing a source distribution. Only the empty `.env.example` is publishable.
- Retain complete tool arguments and answer text. Test fixtures must not become hardcoded recipes or silently rewrite model-generated commands.
- Keep archive/build contexts on explicit allowlists and record real test results and skips.
- Central policy administration and multi-user acceptance remain separate future work; the repository split does not implement them.
