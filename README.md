# spoke-foundryvtt

<!--
==============================================================================
README.md - spoke-foundryvtt module documentation
==============================================================================
Description: FoundryVTT virtual tabletop gaming platform Spoke module
Author: Matt Barham
Created: 2026-02-12
Modified: 2026-04-21
Version: 1.0.1
==============================================================================
Document Type: Reference
Audience: Developer
Status: Final
==============================================================================
-->

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/E1E21U3S1R)

Spoke module for [FoundryVTT](https://foundryvtt.com/) virtual tabletop gaming platform.

## Services

| Service    | Description                  | Port  | Network |
|------------|------------------------------|-------|---------|
| foundryvtt | FoundryVTT web application   | 30000 | troxy   |

## Prerequisites

- Spoke hub deployed with `troxy` network
- Traefik available as a hub service
- FoundryVTT license key (stored in secrets as `foundryvtt_secrets.json`)

## Quick Start

```bash
# Copy and configure environment
cp .env.example .env
# Edit .env with your values

# Create secrets directory and add license config
mkdir -p ${SECRETS_DIR}/foundryvtt/
# Place your foundryvtt_secrets.json there (contains license key + admin password)

# Deploy
docker compose up -d
```

## Module Environment Variables

| Variable              | Default                      | Description                    |
|-----------------------|------------------------------|--------------------------------|
| `FOUNDRYVTT_IMAGE`    | `felddy/foundryvtt:14.364.0` | Container image                |
| `FOUNDRYVTT_IP`       | `192.168.35.25`              | Static IP on troxy network     |
| `FOUNDRYVTT_HOST`     | `foundry.${DOMAIN}`          | FoundryVTT public hostname     |
| `FOUNDRYVTT_DATA_DIR` | `${APPDATA_DIR}/foundryvtt/data` | Persistent data directory  |

## Secrets

| Secret Name                | Required | Description                           |
|----------------------------|----------|---------------------------------------|
| `foundryvtt_secrets_json`  | Yes      | FoundryVTT config.json with license key and admin password |

The `foundryvtt_secrets.json` file is mounted as `config.json` inside the container. It should contain your FoundryVTT license key and admin credentials.

## Traefik Routing

Dynamic rules in `traefik/` are deployed by the hub:

- `routers_foundryvtt.yml` - Routes `foundry.<DOMAIN>` (no auth, FoundryVTT has built-in auth)
- `services_foundryvtt.yml` - Load balancer pointing to `foundryvtt:30000`

## References

- [FoundryVTT](https://foundryvtt.com/)
- [felddy/foundryvtt-docker](https://github.com/felddy/foundryvtt-docker)
