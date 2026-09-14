# MTA-STS Deployment Template (RFC 8461)

A lightweight static policy template for deploying **SMTP MTA Strict Transport Security (MTA-STS)** via Cloudflare Pages or any static web host.

## Overview

MTA-STS (RFC 8461) secures inbound email by declaring that sending mail transfer agents must use TLS encryption with valid certificates, protecting against downgrade attacks and Man-in-the-Middle (MITM) interception.

## Structure

- **`public/.well-known/mta-sts.txt`**: The authoritative policy file served over HTTPS at `mta-sts.yourdomain.com/.well-known/mta-sts.txt`.

## Policy Configuration

```text
version: STSv1
mode: testing        # Switch to 'enforce' once verified
mx: mx.windowsforum.com
mx: aspmx.l.google.com
mx: alt1.aspmx.l.google.com
mx: alt2.aspmx.l.google.com
mx: alt3.aspmx.l.google.com
mx: alt4.aspmx.l.google.com
max_age: 1209600
```

## DNS Setup

To activate MTA-STS, configure the following DNS records on your domain:

1. **CNAME Record** (Host the policy):
   - Name: `mta-sts`
   - Target: `<your-cloudflare-pages-project>.pages.dev` (or static host with valid SSL)
2. **TXT Record** (Policy discovery):
   - Name: `_mta-sts`
   - Value: `v=STSv1; id=2026090101;` *(increment `id` when policy changes)*
3. **TXT Record** (TLS Reporting / RFC 8460):
   - Name: `_smtp._tls`
   - Value: `v=TLSRPTv1; rua=mailto:tls-reports@example.com`

## Deployment
Connect this repository to **Cloudflare Pages** with the build output directory set to `public`.