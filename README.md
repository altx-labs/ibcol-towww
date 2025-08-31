# IBCOL Domain Redirect Service

[![ZEIT Now Deployment](https://img.shields.io/badge/ZEIT%20Now-deployed-brightgreen.svg)](https://zeit.co/now)
[![Version](https://img.shields.io/badge/version-1.0.1-blue.svg)](package.json)

A lightweight domain redirect service for the International Blockchain Olympiad (IBCOL) website. This service redirects traffic from the apex domain (`ibcol.org`) to the www subdomain (`www.ibcol.org`).

## Overview

This repository contains a simple, yet effective domain redirection service built using [ZEIT Now](https://zeit.co/now) (now Vercel). It ensures that all traffic to the apex domain is properly redirected to the www subdomain with appropriate HTTP status codes.

## Features

- **301 Permanent Redirects**: Ensures search engines update their indexes
- **Path Preservation**: Maintains URL paths during redirection
- **Global Deployment**: Deployed across all regions for low-latency redirects
- **Zero Maintenance**: Serverless architecture requires no ongoing maintenance

## Technical Implementation

The service uses ZEIT Now's routing configuration to handle redirects:

```json
{
  "version": 2,
  "name": "ibcol-towww",
  "alias": ["ibcol.org"],
  "regions": ["all"],
  "routes": [
    {
      "src": "/(.*)",
      "status": 301,
      "headers": { "Location": "https://www.ibcol.org/$1" }
    }
  ]
}
```

This configuration:
1. Captures all incoming requests to any path on the apex domain
2. Returns a 301 (Permanent Redirect) status code
3. Preserves the original path in the redirect URL
4. Directs users to the corresponding path on the www subdomain

## Deployment

The service is deployed using the ZEIT Now CLI:

```bash
now --team ibcol && now alias --team ibcol
```

This command deploys the service and sets up the domain alias in a single step.

## Related Projects

The IBCOL ecosystem consists of several interconnected projects:

- [ibcol](https://github.com/altx-labs/ibcol) - Main website for the International Blockchain Olympiad
- [ibcol-micro-graphql-api](https://github.com/altx-labs/ibcol-micro-graphql-api) - GraphQL API service for IBCOL applications

## About IBCOL

The International Blockchain Olympiad (IBCOL) is an annual competition founded in 2017 as a platform to boost technical excellence among youths in blockchain and trust technologies. It provides a space for students to showcase innovative blockchain solutions to real-world problems.

IBCOL's mission is to promote "Blockchain Without Bullsh*t" - focusing on the technical merits and practical applications of blockchain technology rather than speculative aspects.

## License

This project is proprietary and not licensed for public use.

## Authors

- **Breaking Bad Interactive** - [bbi.io](http://bbi.io)
- **William Li** - [williamli.io](http://williamli.io)

