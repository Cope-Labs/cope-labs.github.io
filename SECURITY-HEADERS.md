# Cloudflare security headers for copelabs.dev

Set these in Cloudflare dashboard → copelabs.dev → Rules → Transform Rules → Modify Response Header.

## Headers to add

| Header | Value |
|--------|-------|
| X-Content-Type-Options | nosniff |
| X-Frame-Options | DENY |
| Referrer-Policy | strict-origin-when-cross-origin |
| Permissions-Policy | camera=(), microphone=(), geolocation=(), interest-cohort=() |
| Strict-Transport-Security | max-age=31536000; includeSubDomains; preload |
| X-XSS-Protection | 0 |
| Content-Security-Policy | default-src 'none'; style-src 'unsafe-inline'; img-src 'self'; script-src 'self' static.cloudflareinsights.com; connect-src static.cloudflareinsights.com; font-src 'none'; frame-src 'none'; base-uri 'self'; form-action 'none'; |

## What each does

- **X-Content-Type-Options: nosniff** — prevents MIME-type sniffing attacks
- **X-Frame-Options: DENY** — prevents clickjacking (nobody should iframe this)
- **Referrer-Policy** — don't leak full URLs to third parties
- **Permissions-Policy** — disable browser APIs we don't use (camera, mic, location, FLoC)
- **HSTS** — force HTTPS always, tell browsers to never try HTTP
- **X-XSS-Protection: 0** — disabled because CSP handles this better (the old XSS filter caused bugs)
- **CSP** — only allow inline styles, self-hosted images, Cloudflare analytics script. Block everything else.

## To set in Cloudflare

1. dash.cloudflare.com → copelabs.dev
2. Rules → Transform Rules
3. Create rule → Modify Response Header
4. Match: all incoming requests
5. Add each header above as "Set static" → header name → value
6. Deploy

## Test after setting

```bash
curl -I https://copelabs.dev
```

All headers should appear in the response.
