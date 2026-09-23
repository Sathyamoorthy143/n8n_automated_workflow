# n8n in GitHub Codespaces

This repository can run n8n and PostgreSQL inside a GitHub Codespace.

## Start

1. Create/open the Codespace from the `codespaces-n8n-cloudflare` branch.
2. The devcontainer creates local data directories and starts Docker Compose.
3. Open forwarded port 5678 to reach n8n.

### First-run secret

Before the first n8n start, set a strong, stable `N8N_ENCRYPTION_KEY` in `.env`.

Then run:

```bash
docker compose up -d
docker compose ps
```

View logs:

```bash
docker compose logs -f n8n
```

## Data

n8n data is stored in `.data/n8n`.
PostgreSQL data is stored in `.data/postgres`.

These directories are intentionally ignored by Git.

## Automatic recovery

Both containers use:

```yaml
restart: unless-stopped
```

The devcontainer also runs:

```text
docker compose up -d
```

after the Codespace starts.

## Cloudflare Tunnel

The intended production-facing hostname is:

```text
https://learnwithsathya.qzz.io
```

Cloudflare Tunnel should forward that hostname to the n8n service on port 5678.

For the tunnel-facing configuration, update the corresponding values in `.env`:

```env
N8N_HOST=learnwithsathya.qzz.io
N8N_PROTOCOL=https
N8N_SECURE_COOKIE=true
N8N_EDITOR_BASE_URL=https://learnwithsathya.qzz.io/
WEBHOOK_URL=https://learnwithsathya.qzz.io/
```

Do not commit Cloudflare credentials or the production `.env`.

## Codespaces availability

Docker restart policies can recover services while the Codespace is running, but they cannot make a Codespace a guaranteed 24/7 server. Codespaces may stop or be reclaimed according to GitHub's current policies and account limits.

For true always-on operation, move the same Docker Compose stack to a persistent VM/server.

## Useful commands

```bash
docker compose up -d
docker compose stop
docker compose start
docker compose restart
docker compose ps
docker compose logs -f n8n
```
