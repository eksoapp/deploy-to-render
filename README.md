# Deploy Ekso to Render

[Ekso](https://ekso.app) is a self-hosted ticketing, help desk, project management, resource planning, and time tracking app. This repo is the [Render Blueprint](https://render.com/docs/infrastructure-as-code) that deploys Ekso to [Render](https://render.com): managed Postgres 18 with pgvector, a web service for the API, and a background worker.

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/EksoApp/Deploy-to-Render)

## Before you click

You'll need:

- A **Render account** with billing set up. The smallest viable shape (Starter web + Starter worker + Starter Postgres) is roughly **$21–26 / month**.
- An **S3-compatible bucket** with an access key for uploaded attachments. Cloudflare R2 (zero egress fees) is the recommended default — AWS S3, Backblaze B2, and MinIO also work.
- A **free Ekso license** from [ekso.app/get-started](https://ekso.app/get-started) (free tier is unlimited users, no credit card).

When you click the button, Render prompts for four bucket fields (`Storage__S3Bucket`, `Storage__S3Endpoint`, `Storage__S3AccessKey`, `Storage__S3SecretKey`) — paste them from your bucket provider's API token page. The JWT signing key is auto-generated; you don't set it manually.

## Full walkthrough

**[ekso.dev/guide/install/render](https://ekso.dev/guide/install/render)** — prerequisites, per-provider bucket setup (R2 / AWS / B2 / MinIO), cost shape, backups, custom domain, updating, troubleshooting.

For the Docker compose path (run on your own machine instead of Render), see [ekso.dev/guide/install/quickstart](https://ekso.dev/guide/install/quickstart).

## What you get

| Render resource | Image / type | Role |
|---|---|---|
| `ekso-api` | Web Service from `eksoapp/app:latest` | REST API + UI behind Render HTTPS |
| `ekso-worker` | Background Worker from `eksoapp/worker:latest` | Background jobs — email, vector indexing |
| `ekso-db` | Managed Postgres 18 + pgvector | OLTP + reporting database |

Plus your own S3-compatible bucket (lives in your cloud provider's account, not Render's) for attachments.

## About this repo

This is a published mirror of the canonical `render.yaml` shipped in the [`ekso-render.zip`](https://ekso.app/download/ekso-render.zip) bundle. It's updated automatically on every Ekso release.

If you want to customise the Blueprint (different region, pinned image version, extra envvars, larger plan), **fork this repo** rather than open a PR — your fork's `render.yaml` is yours to edit, and Render will deploy from your fork going forward. PRs against this repo will be overwritten by the next mirror sync.

## License

The Blueprint files in this repo are MIT-licensed. The Ekso application itself is licensed separately — see [ekso.app/pricing](https://ekso.app/pricing).
