# System Architecture

This document outlines the system architecture of the IBCOL Domain Redirect Service.

## Architecture Overview

The IBCOL Domain Redirect Service is built on a serverless architecture using ZEIT Now (now Vercel). This approach provides several advantages:

- **Zero Infrastructure Management**: No servers to maintain or scale
- **Global Edge Network**: Automatic deployment to edge locations worldwide
- **High Availability**: Built-in redundancy and fault tolerance
- **Automatic Scaling**: Handles traffic spikes without manual intervention

## System Components

The system consists of the following key components:

### 1. ZEIT Now Configuration

The core of the service is the `now.json` configuration file, which defines:
- The deployment version (v2)
- Project name
- Domain aliases
- Deployment regions
- Routing rules

```
┌─────────────────┐
│                 │
│    now.json     │
│  Configuration  │
│                 │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│                 │
│   ZEIT Now      │
│   Platform      │
│                 │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│                 │
│  Global Edge    │
│    Network      │
│                 │
└─────────────────┘
```

### 2. Routing Rules

The routing configuration is the heart of the service:

```json
"routes": [
  {
    "src": "/(.*)",
    "status": 301,
    "headers": { "Location": "https://www.ibcol.org/$1" }
  }
]
```

This configuration:
1. Captures all incoming requests using the `/(.*)`  pattern
2. Returns a 301 (Permanent Redirect) status code
3. Sets the `Location` header to the www subdomain while preserving the path

### 3. DNS Configuration

For the redirect to work properly, DNS records must be configured correctly:

- **A Record**: Points `ibcol.org` to ZEIT Now's edge network
- **CNAME Record**: Points `www.ibcol.org` to the main IBCOL website hosting

## Request Flow

When a user visits the apex domain, the following sequence occurs:

1. User navigates to `ibcol.org/some-path`
2. DNS resolves to ZEIT Now's edge network
3. The request is routed to the nearest edge location
4. The routing rule matches the request pattern
5. A 301 redirect response is generated with `Location: https://www.ibcol.org/some-path`
6. The user's browser automatically follows the redirect to the www subdomain
7. The www subdomain serves the requested content

```
┌──────────┐    1. Request    ┌──────────┐
│          │ ───────────────► │          │
│  User    │                  │  DNS     │
│ Browser  │ ◄─────────────── │          │
└──────────┘    2. IP Address └──────────┘
      │
      │ 3. HTTP Request
      ▼
┌──────────────────────┐
│                      │
│  ZEIT Now Edge Node  │
│                      │
└──────────┬───────────┘
           │
           │ 4. 301 Redirect
           ▼
┌──────────────────────┐
│                      │
│  www.ibcol.org       │
│  (Main Website)      │
│                      │
└──────────────────────┘
```

## Performance Considerations

The redirect service is designed for optimal performance:

- **Minimal Latency**: Edge deployment ensures requests are handled close to the user
- **Lightweight Response**: The redirect response is small and processed quickly
- **No Server-Side Processing**: No computation is needed to generate the response
- **Caching**: Browsers typically cache 301 redirects, reducing subsequent redirect requests

## Security Considerations

Although the service is simple, several security aspects are addressed:

- **HTTPS Only**: All redirects point to HTTPS URLs
- **No User Data Processing**: The service doesn't collect or process user data
- **No Attack Surface**: With no server-side code execution, the attack surface is minimal
- **Platform Security**: Leverages ZEIT Now's built-in security features

## Integration with IBCOL Ecosystem

The redirect service is part of the larger IBCOL web ecosystem:

```
┌─────────────────────┐      ┌─────────────────────┐
│                     │      │                     │
│  ibcol-towww        │─────►│  ibcol              │
│  (Redirect Service) │      │  (Main Website)     │
│                     │      │                     │
└─────────────────────┘      └──────────┬──────────┘
                                        │
                                        │
                                        ▼
                             ┌─────────────────────┐
                             │                     │
                             │  ibcol-micro-       │
                             │  graphql-api        │
                             │                     │
                             └─────────────────────┘
```

This architecture ensures a seamless user experience while maintaining a clean separation of concerns between different services.

