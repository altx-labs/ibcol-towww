# Deployment Process

This document outlines the deployment process for the IBCOL Domain Redirect Service.

## Deployment Overview

The IBCOL Domain Redirect Service is deployed using ZEIT Now (Vercel), a serverless deployment platform. The deployment process is straightforward and can be performed using the ZEIT Now CLI.

## Prerequisites

Before deploying, ensure you have:

1. **ZEIT Now CLI**: Install the CLI tool using npm:
   ```bash
   npm install -g now
   ```

2. **ZEIT Now Account**: Create an account at [zeit.co](https://zeit.co) if you don't have one.

3. **Team Access**: Ensure you have access to the `ibcol` team in ZEIT Now.

4. **Domain Access**: Verify you have permission to deploy to the `ibcol.org` domain.

## Deployment Steps

### 1. Clone the Repository

```bash
git clone https://github.com/altx-labs/ibcol-towww.git
cd ibcol-towww
```

### 2. Authenticate with ZEIT Now

```bash
now login
```

Follow the prompts to authenticate your account.

### 3. Deploy the Service

The deployment is handled by a single command defined in the `package.json` file:

```bash
npm run deploy
```

This command executes:

```bash
now --team ibcol && now alias --team ibcol
```

Which performs two actions:
1. Deploys the service to ZEIT Now under the `ibcol` team
2. Sets up the domain alias according to the `alias` field in `now.json`

### 4. Verify the Deployment

After deployment, verify that the redirect is working correctly:

1. Visit [ibcol.org](https://ibcol.org)
2. Confirm you are redirected to [www.ibcol.org](https://www.ibcol.org)
3. Check that the HTTP status code is 301 (you can use browser developer tools for this)

## Deployment Workflow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│             │     │             │     │             │
│  Local      │────►│  ZEIT Now   │────►│  Production │
│  Development│     │  Build      │     │  Deployment │
│             │     │             │     │             │
└─────────────┘     └─────────────┘     └─────────────┘
```

## Continuous Integration

While not currently implemented, the service could benefit from continuous integration:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│             │     │             │     │             │     │             │
│  Git        │────►│  CI/CD      │────►│  Automated  │────►│  Production │
│  Repository │     │  Pipeline   │     │  Tests      │     │  Deployment │
│             │     │             │     │             │     │             │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
```

## Deployment Environments

The service can be deployed to different environments by modifying the deployment command:

### Development Environment

```bash
now --team ibcol
```

This deploys to a unique URL without setting up the production domain alias.

### Production Environment

```bash
now --team ibcol && now alias --team ibcol
```

This deploys and sets up the production domain alias.

## Rollback Process

If a deployment causes issues, you can roll back to a previous version:

1. List recent deployments:
   ```bash
   now ls --team ibcol
   ```

2. Alias a previous deployment:
   ```bash
   now alias [deployment-url] ibcol.org --team ibcol
   ```

## Deployment Monitoring

After deployment, monitor the service using:

1. **ZEIT Now Dashboard**: Visit the [ZEIT Now Dashboard](https://zeit.co/dashboard) to view deployment status and logs.

2. **HTTP Status Monitoring**: Set up external monitoring to periodically check that the redirect is working correctly.

## Deployment Security

The deployment process includes several security considerations:

1. **Team-Based Access Control**: Only members of the `ibcol` team can deploy to the domain.

2. **No Secrets in Code**: The configuration contains no sensitive information.

3. **HTTPS by Default**: ZEIT Now enforces HTTPS for all deployments.

4. **Immutable Deployments**: Each deployment creates an immutable snapshot, enabling easy rollbacks.

## Troubleshooting Deployments

If you encounter issues during deployment:

1. **Check ZEIT Now Status**: Visit [status.zeit.co](https://status.zeit.co) to check if there are platform issues.

2. **Verify Team Access**: Ensure you have the correct permissions within the `ibcol` team.

3. **Check Domain Configuration**: Verify that the domain is properly configured in the ZEIT Now dashboard.

4. **Review Deployment Logs**: Check the logs in the ZEIT Now dashboard for error messages.

5. **Validate Configuration**: Ensure the `now.json` file is valid JSON and follows the correct schema.

