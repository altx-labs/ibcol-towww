# Frequently Asked Questions (FAQ)

This document answers common questions about the IBCOL Domain Redirect Service.

## General Questions

### What is the IBCOL Domain Redirect Service?

The IBCOL Domain Redirect Service is a lightweight serverless application that redirects visitors from the apex domain (`ibcol.org`) to the www subdomain (`www.ibcol.org`). It ensures a consistent user experience and proper SEO handling.

### Why is a redirect service needed?

A redirect service is needed for several reasons:

1. **SEO Consolidation**: Prevents splitting SEO value between two versions of the same site
2. **Consistent User Experience**: Ensures users always see the site at the same URL
3. **Technical Requirements**: Some web hosting services work better with www subdomains
4. **Analytics Accuracy**: Prevents splitting analytics data between different domain versions

### How does the redirect service work?

The service uses ZEIT Now's (Vercel's) routing capabilities to:

1. Capture all requests to `ibcol.org` and any path under it
2. Return an HTTP 301 (Permanent Redirect) status code
3. Set the `Location` header to the corresponding URL on `www.ibcol.org`
4. The user's browser automatically follows the redirect

## Technical Questions

### What technology stack is used?

The service uses:
- **ZEIT Now** (Vercel): Serverless deployment platform
- **JSON Configuration**: Simple configuration-based routing
- **DNS**: Standard DNS records to point the domain to ZEIT Now

### Is there any server-side code?

No, the service is entirely configuration-based. There is no server-side code to execute, which makes it:
- Extremely reliable
- Very fast
- Maintenance-free
- Highly secure

### What HTTP status code is used for the redirect?

The service uses HTTP status code 301 (Moved Permanently). This is the appropriate code for permanent redirects and has several advantages:
- Search engines update their indexes to use the new URL
- Browsers may cache the redirect, improving performance for repeat visitors
- It clearly indicates that the content has permanently moved

### Does the service preserve URL paths?

Yes, the service preserves the full path when redirecting. For example:
- `ibcol.org/about` redirects to `www.ibcol.org/about`
- `ibcol.org/events/2023` redirects to `www.ibcol.org/events/2023`

This is achieved using a capture group in the route pattern and a reference in the redirect URL.

## Operational Questions

### How is the service deployed?

The service is deployed using the ZEIT Now CLI with a single command:

```bash
now --team ibcol && now alias --team ibcol
```

This deploys the service and sets up the domain alias in one step.

### How much does the service cost to operate?

The service operates within ZEIT Now's free tier limits because:
- It handles a moderate amount of traffic
- It requires no computing resources (just configuration)
- It uses minimal bandwidth (just HTTP headers)

### How is the service monitored?

The service can be monitored through:
- ZEIT Now's built-in analytics
- External uptime monitoring services
- Periodic manual checks

### What happens if the service goes down?

If the service experiences an outage:
- Users would not be redirected from `ibcol.org` to `www.ibcol.org`
- They would see a ZEIT Now error page instead
- The main website at `www.ibcol.org` would still function normally

However, because the service is so simple and runs on ZEIT Now's reliable infrastructure, outages are extremely unlikely.

## DNS and Domain Questions

### What DNS records are needed for the redirect service?

The following DNS records are required:
- **A Record**: Points `ibcol.org` to ZEIT Now's IP addresses
- **CNAME Record**: Points `www.ibcol.org` to the main website hosting (not part of this service)

### Can the service handle subdomains?

The current configuration only handles the apex domain (`ibcol.org`). Subdomains would need their own configuration, either:
- Additional entries in the `alias` array in `now.json`
- Separate redirect services for each subdomain
- Wildcard DNS and more complex routing rules

### Can the service redirect to different targets based on the path?

While the current implementation redirects all paths to the same target domain, it would be possible to modify the configuration to redirect different paths to different targets. This would require additional route entries in the `now.json` file.

## Security Questions

### Is the redirect service secure?

Yes, the service is highly secure because:
- It contains no executable code, eliminating code-based vulnerabilities
- It runs on ZEIT Now's secure infrastructure
- It uses HTTPS for all redirects
- It doesn't process or store any user data

### Does the service handle HTTPS?

Yes, ZEIT Now automatically provisions SSL certificates and ensures all traffic uses HTTPS. This means:
- Connections to `ibcol.org` are encrypted
- Redirects always point to the HTTPS version of `www.ibcol.org`

### Does the service collect or store any user data?

No, the service doesn't collect or store any user data. It simply issues a redirect response without processing the request beyond matching the URL pattern.

## Maintenance Questions

### How often does the service need to be updated?

The service requires minimal maintenance:
- Updates to the ZEIT Now platform are handled automatically
- The configuration only needs to be updated if the redirect target changes
- DNS records should be reviewed periodically to ensure they're correct

### Who is responsible for maintaining the service?

The service is maintained by the IBCOL technical team, specifically those with access to:
- The GitHub repository
- The ZEIT Now `ibcol` team
- The domain's DNS settings

### How can I contribute to improving the service?

If you'd like to contribute:
1. Fork the GitHub repository
2. Make your changes
3. Submit a pull request with a clear description of the improvements
4. The maintainers will review your contribution

