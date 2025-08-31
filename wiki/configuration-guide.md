# Configuration Guide

This guide explains the configuration options for the IBCOL Domain Redirect Service.

## Configuration File: now.json

The entire service is configured through the `now.json` file. This file follows the ZEIT Now (Vercel) configuration format and contains all the settings needed for the redirect service to function.

### Basic Structure

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

### Configuration Options

#### version

```json
"version": 2
```

This specifies the version of the ZEIT Now platform to use. Version 2 is the current standard and provides the routing capabilities needed for the redirect service.

#### name

```json
"name": "ibcol-towww"
```

The project name used within the ZEIT Now platform. This helps identify the project in the ZEIT dashboard and CLI.

#### alias

```json
"alias": ["ibcol.org"]
```

This defines the domain that will be associated with the deployment. When using the `now alias` command, this domain will be linked to the latest deployment.

#### regions

```json
"regions": ["all"]
```

This specifies which geographical regions the service should be deployed to. The value `"all"` ensures the service is deployed globally for the lowest possible latency.

Options include:
- `"all"` - Deploy to all regions
- Specific regions like `["sfo1", "bru1"]` - Deploy only to San Francisco and Brussels

#### routes

```json
"routes": [
  {
    "src": "/(.*)",
    "status": 301,
    "headers": { "Location": "https://www.ibcol.org/$1" }
  }
]
```

The routes array defines how incoming requests should be handled. Each route object can have the following properties:

- **src**: A pattern to match against the incoming request path
- **status**: The HTTP status code to return
- **headers**: HTTP headers to include in the response

### Route Pattern Explanation

The pattern `/(.*)`  is a regular expression that:
- Matches the root path `/` and any characters that follow
- Captures the entire path in a capture group
- The captured value can be referenced as `$1` in the redirect URL

### Status Code Selection

The service uses HTTP status code 301 (Moved Permanently) rather than 302 (Found) or 307 (Temporary Redirect) because:

- 301 indicates to browsers and search engines that the redirect is permanent
- Search engines will update their indexes to use the new URL
- Browsers will cache the redirect, potentially skipping the redirect request on subsequent visits

## Advanced Configuration Options

While not currently used in this service, ZEIT Now supports additional configuration options that could be added if needed:

### Environment Variables

```json
"env": {
  "TARGET_DOMAIN": "www.ibcol.org"
}
```

This would allow the target domain to be configured as an environment variable.

### Build Settings

```json
"builds": [
  { "src": "*.js", "use": "@now/node" }
]
```

This would enable server-side code execution if more complex redirect logic were needed.

### Headers

```json
"headers": [
  {
    "source": "/(.*)",
    "headers": [
      {
        "key": "Cache-Control",
        "value": "s-maxage=1, stale-while-revalidate"
      }
    ]
  }
]
```

This would add custom headers to all responses.

## Configuration Best Practices

1. **Keep It Simple**: For a redirect service, simpler is better. The current configuration is minimal but complete.

2. **Use Permanent Redirects**: 301 redirects are appropriate for domain redirects that won't change.

3. **Preserve Paths**: The capture group and reference ensures users arrive at the correct page after redirection.

4. **Deploy Globally**: Using `"regions": ["all"]` ensures low latency for all users worldwide.

5. **Version Control**: Keep the configuration in version control to track changes and enable rollbacks if needed.

## Testing Configuration Changes

Before deploying configuration changes to production, you can test them using:

```bash
now dev
```

This will start a local development server that simulates the ZEIT Now environment.

