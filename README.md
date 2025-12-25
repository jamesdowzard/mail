# Mail Infrastructure

Self-hosted email for `dowzard.family` using [Stalwart Mail Server](https://stalw.art/).

## Current Status

| Component | Status |
|-----------|--------|
| Stalwart Container | Running on Pi |
| Domain Config | dowzard.family configured |
| User Account | james@dowzard.family |
| IMAP Access | Working (internal) |
| DNS Records | MX, SPF, DMARC, DKIM configured |
| Port Forwarding | DMZ to Pi (ISP may block port 25) |
| AWS SES | Domain verified, DKIM active |
| External Sending | Pending (configure relay in Stalwart) |
| External Receiving | Pending (Cloudflare Email Routing) |

## Quick Start

```bash
# Deploy to Pi
scp -r docker/* pi:~/stalwart/
ssh pi "cd ~/stalwart && docker compose up -d"

# Test IMAP
python brain/client.py test
```

## Architecture

```
Internet → Cloudflare (MX) → Router (NAT) → Pi (Stalwart)
                                              ├── SMTP (25, 587)
                                              ├── IMAP (993)
                                              └── Admin (8093)
```

## Implementation Plan

See `.harness/state.json` for detailed feature tracking.

### Phase 1: Infrastructure
1. **docker-config** - Finalize docker-compose and config
2. **dns-records** - MX, SPF, DKIM, DMARC in Cloudflare
3. **ssl-certs** - Let's Encrypt for mail.dowzard.family
4. **port-forwarding** - Router config for mail ports

### Phase 2: Email Flow
5. **sending-config** - Outbound email (relay or direct)
6. **spam-filtering** - Inbound spam protection

### Phase 3: Integration
7. **brain-client** - Python API for Brain
8. **monitoring** - ntfy alerts for health
9. **backup** - S3 backup automation

## Credentials

Stored in `~/.credentials/stalwart.env`:
- Admin password
- User password
- SMTP relay credentials (if used)

## License

Private - Personal infrastructure
