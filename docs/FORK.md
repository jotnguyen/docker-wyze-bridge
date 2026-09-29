# About this fork

This is a fork of [mrlt8/docker-wyze-bridge](https://github.com/mrlt8/docker-wyze-bridge), which
has not changed since 2024-09. It stays on the 2.x Python bridge because 2.x can reach cameras
remotely through Wyze's P2P relay (`NET_MODE=ANY`). The maintained v4 rewrite is LAN-only.

## Changes from upstream

- **No secrets in logs.** The startup log prints the `WB_API` token and the MediaMTX stream
  password as `***` (`[AUTH] WB_API=***`, `[MTX] Auth [<user>:***]`).
- **Images are published to GHCR only:** `ghcr.io/jotnguyen/docker-wyze-bridge`, multiarch
  (amd64 + arm64). A push to `main` publishes `:edge`. A tag `vX.Y.Z-<suffix>.N` publishes
  `:X.Y.Z-<suffix>.N`.

## Wyze app-version string

Wyze sometimes rejects old app versions. When that happens, set `APP_VERSION` (and `IOS_VERSION`,
if needed) on the container; no rebuild is needed. `load_dotenv()` never overrides variables that
are already set, so the container value wins over `app/.env`.

## Picking up upstream fixes

```bash
git remote add upstream https://github.com/mrlt8/docker-wyze-bridge   # once
git fetch upstream --tags
git cherry-pick <sha>
```

After the cherry-pick, push a new tag so CI publishes an image.
