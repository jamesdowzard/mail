# Mail Infrastructure

Self-hosted email system for dowzard.family using Stalwart Mail Server on Pi.

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    INTERNET                              │
└─────────────────────────────────────────────────────────┘
                          │
                   ┌──────┴──────┐
                   │  Cloudflare  │
                   │   (DNS/MX)   │
                   └──────┬──────┘
                          │
              ┌───────────┴───────────┐
              │   Router (Port Fwd)   │
              │  25, 465, 587, 993    │
              └───────────┬───────────┘
                          │
              ┌───────────┴───────────┐
              │     Pi (Stalwart)     │
              │  - SMTP (25, 587)     │
              │  - IMAP (993)         │
              │  - Admin (8093)       │
              └───────────────────────┘
```

## Components

| Component | Location | Purpose |
|-----------|----------|---------|
| `docker/` | Pi deployment | Docker compose for Stalwart |
| `dns/` | Cloudflare records | MX, SPF, DKIM, DMARC |
| `scripts/` | Automation | Setup, backup, monitoring |
| `brain/` | Brain integration | Python client for email ops |

## Stalwart Server

- **Admin URL:** http://192.168.0.58:8093
- **IMAP:** 192.168.0.58:993
- **SMTP:** 192.168.0.58:587
- **Domain:** dowzard.family
- **User:** james@dowzard.family

## Credentials

Stored in `~/.credentials/stalwart.env` - never commit to repo.

## Current Status

Running parallel with Gmail for testing. See `.harness/state.json` for implementation progress.
