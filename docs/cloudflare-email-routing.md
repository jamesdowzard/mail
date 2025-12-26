# Cloudflare Email Routing

## Status: COMPLETE

Inbound email for dowzard.family is handled by Cloudflare Email Routing since the ISP blocks port 25.

## Routing Rules

| Custom Address | Action | Destination | Status |
|---------------|--------|-------------|--------|
| james@dowzard.family | Forward | james.dowzard@gmail.com | Active |

## DNS Records (Auto-configured)

Cloudflare Email Routing automatically manages these records:

### MX Records
```
dowzard.family  MX  69  route1.mx.cloudflare.net
dowzard.family  MX  89  route2.mx.cloudflare.net
dowzard.family  MX  79  route3.mx.cloudflare.net
```

### TXT Records
```
dowzard.family                      TXT  "v=spf1 include:_spf.mx.cloudflare.net ~all"
cf2024-1._domainkey.dowzard.family  TXT  "v=DKIM1; h=sha256; ..."
```

## How It Works

1. Email sent to james@dowzard.family
2. MX records point to Cloudflare's mail servers
3. Cloudflare receives the email
4. Email is forwarded to james.dowzard@gmail.com
5. Appears in Gmail inbox (may be in a folder based on Gmail filters)

## Managing Routes

- Dashboard: https://dash.cloudflare.com > dowzard.family > Email > Email Routing
- Add new addresses via "Create address" button
- Catch-all is disabled (unmatched addresses are dropped)

## Limitations

- Maximum 200 destination addresses per zone
- Maximum 5 MB email size
- Headers may be modified (X-Forwarded-For added)
- Reply-to address will show the original sender

## Testing

Send a test email from any external address to james@dowzard.family and verify it arrives in Gmail.
