# umbrel-opencode

Umbrel packaging for **[OpenCode](https://opencode.ai/)** (Anomaly): a **setup page** in the browser (`web` / nginx) plus an **`opencode`** service using the **official** multi-arch image [`ghcr.io/anomalyco/opencode`](https://github.com/anomalyco/opencode/pkgs/container/opencode). Persistent data lives under `${APP_DATA_DIR}/data` → `/data` in the container (`HOME=/data`, including `data/workspace/` for projects).

There is **no in-browser TUI**; use Umbrel **Terminal** or `docker exec -it` (commands on the setup page).

## Canonical install surface

Add the community store in umbrelOS:

`https://github.com/harmalh/umbrel-community-store`

Install **OpenCode** (`harmalh-opencode`). Sync the store from this repo per [`docs/STORE_SYNC.md`](docs/STORE_SYNC.md) and the [umbrel-community-store release checklist](https://github.com/harmalh/umbrel-community-store/blob/main/docs/RELEASE_CHECKLIST.md).

## Compose layout

- **`app_proxy`** — `APP_HOST` **`harmalh-opencode_web_1`**, port 80.
- **`web`** — nginx serves `${APP_DATA_DIR}/www` (HTML from `hooks/pre-start`).
- **`opencode`** — upstream image on **`beta`** tag; `sleep infinity` so the container stays up; run **`opencode`** via `docker exec`.

Pin a **digest** on `ghcr.io/anomalyco/opencode` when you want reproducible installs (`docker buildx imagetools inspect ...`).

## First-time use on Umbrel

1. Open the app from the dashboard (setup page).
2. **Settings → Advanced → Terminal** (or SSH).
3. Example: `docker exec -it harmalh-opencode_opencode_1 sh -c 'cd /data/workspace && opencode'`
4. Configure providers per [OpenCode docs](https://opencode.ai/docs/) — do **not** commit API keys; do not use Umbrel `APP_SEED` as a third-party key.

## Local / CI notes

- Standalone `docker compose config` fails without Umbrel’s `app_proxy` merge; CI validates YAML only.
- Device testing: [`umbrel-community-store/docs/UMBREL_DEVICE_TESTING.md`](https://github.com/harmalh/umbrel-community-store/blob/main/docs/UMBREL_DEVICE_TESTING.md).

## CI/CD overview

| Workflow | Purpose |
|----------|---------|
| `.github/workflows/ci.yml` | YAML validation on `main` (compose + manifest; no full `compose config`). |

No custom GHCR build is required unless you add a thin wrapper image (e.g. extra tools); this package consumes upstream images directly.

## Official Umbrel App Store

To propose inclusion in [getumbrel/umbrel-apps](https://github.com/getumbrel/umbrel-apps), fork the official repo, adapt `id` / `APP_HOST` to non-prefixed naming, add screenshots and a pinned digest, and open a PR per Umbrel’s guidelines.
