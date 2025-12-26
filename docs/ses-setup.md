# AWS SES Setup for Stalwart Mail

## Status: COMPLETE - Relay Configured

The dowzard.family domain is configured in AWS SES (ap-southeast-2 region) for sending email.

## DNS Records (Already Configured)

### Domain Verification
```
_amazonses.dowzard.family  TXT  "xPOhrpBBJWH8eqqKwVHXXiF9MoCLjZEoqw3j4ruBdAc="
```

### DKIM (3 CNAME records)
```
nvgag5unmzpy64nlfgpk4p2dlbrv3mjn._domainkey.dowzard.family  CNAME  nvgag5unmzpy64nlfgpk4p2dlbrv3mjn.dkim.amazonses.com
6cyiiuhywqv4b2lglo7fubozsc2gd4xz._domainkey.dowzard.family  CNAME  6cyiiuhywqv4b2lglo7fubozsc2gd4xz.dkim.amazonses.com
ypuyjshp2yx7ynqhgfj7yv7rrfc5g6u3._domainkey.dowzard.family  CNAME  ypuyjshp2yx7ynqhgfj7yv7rrfc5g6u3.dkim.amazonses.com
```

## SMTP Credentials

Stored in `~/.credentials/stalwart.env`:

| Setting | Value |
|---------|-------|
| Host | email-smtp.ap-southeast-2.amazonaws.com |
| Port | 587 (STARTTLS) |
| Username | AKIAZKW2QXIE42HGHOIC |
| Password | (see SES_SMTP_PASSWORD in credentials file) |

## Stalwart Configuration (COMPLETE)

SES relay configured in `/opt/stalwart/etc/config.toml` on the Pi:

```toml
# AWS SES SMTP Relay
[queue.route.ses-relay]
type = "relay"
description = "AWS SES SMTP Relay"
address = "email-smtp.ap-southeast-2.amazonaws.com"
port = 587
protocol = "smtp"

[queue.route.ses-relay.tls]
implicit = false

[queue.route.ses-relay.auth]
username = "AKIAZKW2QXIE42HGHOIC"
secret = "<from SES_SMTP_PASSWORD in credentials>"

# Route external mail through SES relay
[queue.outbound]
next-hop = "ses-relay"
```

Verify via API:
```bash
curl -s -u admin:PASSWORD "http://192.168.0.58:8093/api/settings/list?prefix=queue.route.ses-relay" | jq
curl -s -u admin:PASSWORD "http://192.168.0.58:8093/api/settings/list?prefix=queue.outbound" | jq
```

## Verification

After configuration, test sending:

```bash
# Via Stalwart SMTP
echo "Test from Stalwart" | swaks --to test@example.com --from james@dowzard.family \
  --server 192.168.0.58:587 --auth-user james --auth-password <password>
```

## Notes

- SES is in sandbox mode - can only send to verified addresses until production access requested
- Request production access via AWS Console > SES > Account dashboard > Request production access
- Current sending quota: 200 emails/day (sandbox limit)
