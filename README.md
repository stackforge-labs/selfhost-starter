# selfhost-starter

A minimal, pinned Docker Compose setup for self-hosting small services:
Caddy as reverse proxy, with an optional Cloudflare Tunnel so you need no open ports.

## Quick start
```bash
cp .env.example .env
docker compose up -d
curl http://localhost/          # "It works."
curl -H 'Host: whoami.localhost' http://localhost/
```

## Add an app
1. Add the service to `docker-compose.yml` on the `web` network.
2. Add a block to `caddy/Caddyfile`: `http://app.${DOMAIN} { reverse_proxy app:PORT }`
3. `docker compose exec caddy caddy reload --config /etc/caddy/Caddyfile`

## Cloudflare Tunnel (optional)
Create a tunnel in the Cloudflare Zero Trust dashboard, point the public hostname
(and a wildcard) at `http://caddy:80`, put the token in `.env`, then:
```bash
docker compose --profile tunnel up -d
```

## Notes
- Image versions are pinned; bump them deliberately and read release notes.
- Don't expose admin panels publicly; put them behind Cloudflare Access.
- Back up the `caddy_data` volume.

## Want more?
The **Homelab Pack** ($12) adds a tested restic backup compose, an Uptime Kuma setup and a Caddy snippet collection: https://buy.polar.sh/polar_cl_QxmXlV1YqoeC5aTMlqO0o6q6ebpBNGXxLAKyZ20Z86i

Before deploying, lint your compose files with the free [compose-audit](https://github.com/stackforge-labs/compose-audit).

Running AI agents on a box like this? See the free [agent-sandbox-lite](https://github.com/stackforge-labs/agent-sandbox-lite) (egress firewall + tripwire) and the full Agent Sandbox Kit linked from it.

MIT licensed. Maintained by StackForge Labs (an AI-operated project; issues are read by an AI agent).
