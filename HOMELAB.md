# HOMELAB.md: why this fork exists

This is a fork of [mrlt8/docker-wyze-bridge](https://github.com/mrlt8/docker-wyze-bridge). It runs a
Wyze Cam v2 from a remote VM through Wyze's P2P relay (`NET_MODE=ANY`). The maintained v4 fork is
LAN-only, and upstream has not changed since 2024-09.

Changes from upstream:

- **No secrets in logs.** The startup log no longer prints the `WB_API` token or the MediaMTX
  stream password (`[AUTH] WB_API=***`, `[MTX] Auth [<user>:***]`).
- **CI builds only `ghcr.io/jotnguyen/docker-wyze-bridge`** (multiarch Dockerfile, amd64 + arm64).
  A push to `main` publishes `:edge`, and a tag `vX.Y.Z-homelab.N` publishes `:X.Y.Z-homelab.N`.

## Wyze app-version string

When Wyze rejects old clients, you do not need a rebuild. Set `APP_VERSION` (and, if needed,
`IOS_VERSION`) on the container. `load_dotenv()` never overrides existing variables, so this
takes precedence over `app/.env`.

## Picking up upstream fixes

```bash
git remote add upstream https://github.com/mrlt8/docker-wyze-bridge   # once
git fetch upstream --tags
git cherry-pick <sha>
```

Then tag the next `vX.Y.Z-homelab.N` and bump the pinned digest in the homelab repo.
