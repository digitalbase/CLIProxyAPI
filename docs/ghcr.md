# Fork container images

The `Publish GHCR image` workflow publishes `ghcr.io/digitalbase/cliproxyapi`
on every push to `customizations`, on `v*` tags, and on manual runs.
Images support Linux amd64 and arm64. The workflow authenticates with
`GITHUB_TOKEN`; no registry password secret is required.

Tags include `customizations`, `sha-<full-commit>`, and the pushed release tag.
`latest` follows the `customizations` branch.

The image includes the fork's management UI. Its commit is pinned in
`.github/workflows/ghcr.yml`; update that reference when shipping UI changes.
To retain the bundled UI, include this in the mounted configuration:

```yaml
management:
  disable-auto-update-panel: true
```

Run with your configuration and credentials mounted:

```sh
docker run --rm -p 8317:8317 \
  -v "$PWD/config.yaml:/CLIProxyAPI/config.yaml" \
  -v "$PWD/auths:/CLIProxyAPI/auths" \
  ghcr.io/digitalbase/cliproxyapi:latest
```

Set `auth-dir: /CLIProxyAPI/auths` and configure server binding, management
access, and client API keys in `config.yaml`. The image contains no credentials.
New GHCR packages are private by default. Either authenticate before pulling
or change the package visibility to public in GitHub package settings.
