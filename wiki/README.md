# IBCOL Domain Redirect Service Wiki

Welcome to the wiki for the IBCOL Domain Redirect Service. This wiki provides comprehensive documentation about the service's architecture, configuration, and deployment process.

## Table of Contents

- [System Architecture](./system-architecture.md)
- [Configuration Guide](./configuration-guide.md)
- [Deployment Process](./deployment-process.md)
- [Troubleshooting](./troubleshooting.md)
- [FAQ](./faq.md)

## Quick Start

The IBCOL Domain Redirect Service is a simple yet critical component of the IBCOL web infrastructure. It ensures that all traffic to the apex domain (`ibcol.org`) is properly redirected to the www subdomain (`www.ibcol.org`).

### Key Features

- **SEO-Friendly Redirects**: Uses HTTP 301 permanent redirects to maintain search engine rankings
- **Path Preservation**: Maintains the original URL path during redirection
- **Global Deployment**: Deployed across all regions for minimal latency
- **Serverless Architecture**: Zero maintenance overhead with ZEIT Now (Vercel) serverless deployment

### Service Status

The service is currently active and redirecting traffic as expected. You can verify this by visiting [ibcol.org](https://ibcol.org) and observing the redirect to [www.ibcol.org](https://www.ibcol.org).

## Recent Updates

- **Version 1.0.1**: Minor configuration updates and documentation improvements
- **Version 1.0.0**: Initial deployment of the redirect service

## Support

For issues or questions about this service, please contact the IBCOL technical team or create an issue in the repository.

