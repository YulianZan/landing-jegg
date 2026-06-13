# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static landing page for **Team Icarus | REMAX PREMIER**, a Chilean real estate team. The page lets visitors contact agents directly via WhatsApp and links to partner services.

## Stack

- Pure HTML + CSS (single `index.html`, no build tools or JS frameworks)
- Served by **nginx:alpine** via Docker Compose on port **8082**
- Exposed publicly via a **Cloudflare Tunnel** (requires `CLOUDFLARE_TUNNEL_TOKEN` env var)

## Running locally

```bash
# Requires CLOUDFLARE_TUNNEL_TOKEN set in the environment or a .env file at the project root
docker compose up -d

# View logs
docker compose logs -f

# Restart after editing index.html (nginx serves files directly via bind mount — no rebuild needed)
# Just save the file; nginx picks up changes immediately on next request.
```

Stop everything:
```bash
docker compose down
```

## Architecture

```
index.html          ← entire UI: header, agent cards (WhatsApp links), service cards
fondo-ldg.jpg       ← background image referenced by index.html
docker-compose.yml  ← nginx:alpine (port 8082) + cloudflared tunnel
```

The nginx container bind-mounts `index.html` and `fondo-ldg.jpg` as read-only, so edits to `index.html` are served immediately without rebuilding the container.

## Key content locations in index.html

- **Agent cards** (`<div class="agents-grid">`) — each is an `<a>` tag with a `wa.me` WhatsApp link and an agent photo from `gtgluserimages.blob.core.windows.net`
- **Service cards** (`<div class="services-grid">`) — links to `valorraiz.cl` and `jegg.cl`
- **REMAX team profile link** — `https://www.remax.cl/team/1589`
- All copy is in Spanish (Chilean market)
