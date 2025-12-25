# AWS SES Setup for Stalwart Mail

## Status: Domain Verified, DKIM Active

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

## Stalwart Configuration (TODO)

Configure Stalwart to use SES as the SMTP relay:

1. Open Stalwart admin: http://192.168.0.58:8093
2. Login as admin
3. Navigate to: Settings > SMTP > Outbound > Routing
4. Click "+ Create route"
5. Configure:
   - **ID**: `ses-relay`
   - **Type**: Relay Host
   - **Description**: AWS SES SMTP Relay
   - **Address**: `email-smtp.ap-southeast-2.amazonaws.com`
   - **Port**: `587`
   - **Protocol**: SMTP
   - **Implicit TLS**: Off (uses STARTTLS)
   - **Username**: `AKIAZKW2QXIE42HGHOIC`
   - **Secret**: (from SES_SMTP_PASSWORD in credentials)
6. Save changes

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
