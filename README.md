# umbrel-opencode

## Purpose

Placeholder repository for a future **OpenCode** app on **Umbrel**. This repo holds the Umbrel-oriented layout (manifest, compose, CI) until application details are implemented.

## Intended Umbrel app role

Run OpenCode (or your chosen distribution) as an installable Umbrel app with defined networking and persistence. Exact behavior is **TODO**.

## Current status

**Scaffold only** — `docker-compose.yml` and `umbrel-app.yml` are minimal placeholders. Do not treat this as production-ready.

## Local development notes

- Clone this repo and open the folder as its own git root in your editor.
- Replace the `placeholder` service in `docker-compose.yml` with real images and volumes when you have a build.
- Fill in `umbrel-app.yml` (icon URL, tagline, port, description) before publishing to a store.

## Local testing notes

- `docker compose config` should succeed (also run in CI).
- On-device testing on Umbrel: **TODO** after a real stack exists.

## CI/CD overview

| Workflow | Purpose |
|----------|---------|
| `.github/workflows/ci.yml` | On push/PR to `main`: YAML parse for `docker-compose.yml`, `umbrel-app.yml`, and workflows; runs `docker compose config`. |

## Pushing to GitHub

If this is still an empty remote, after your first commit:

```bash
git branch -M main
git push -u origin main
```

## TODO / roadmap

- Define the actual container image, ports, volumes, and Umbrel `app_proxy` expectations.
- Add `.env.example` when configuration surface is known.
- Add image build and GHCR push jobs (see TODO in `ci.yml`).
- Optional: `scripts/` and `docs/` as the app grows.
