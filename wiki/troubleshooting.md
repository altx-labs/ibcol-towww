# Troubleshooting Guide

This guide provides solutions for common issues that may arise with the IBCOL Domain Redirect Service.

## Common Issues and Solutions

### Redirect Not Working

**Symptom**: Visiting `ibcol.org` does not redirect to `www.ibcol.org`.

**Possible Causes and Solutions**:

1. **DNS Configuration Issue**
   - **Check**: Verify that the DNS A record for `ibcol.org` points to ZEIT Now's IP addresses.
   - **Solution**: Update DNS settings with your domain registrar.
   - **Verification**: Run `dig ibcol.org` to check the current DNS settings.

2. **ZEIT Now Configuration Issue**
   - **Check**: Verify that the domain is properly set up in the ZEIT Now dashboard.
   - **Solution**: Go to the ZEIT Now dashboard, select the project, and check domain settings.
   - **Verification**: The domain should show as "Active" in the dashboard.

3. **Deployment Issue**
   - **Check**: Verify that the latest deployment is active and aliased correctly.
   - **Solution**: Run `now ls --team ibcol` to see recent deployments and their status.
   - **Verification**: The most recent deployment should show as "READY" and be aliased to `ibcol.org`.

4. **Configuration Error**
   - **Check**: Verify that the `now.json` file contains the correct routing configuration.
   - **Solution**: Update the `now.json` file with the correct routes and redeploy.
   - **Verification**: The routes section should include a pattern that matches all paths and redirects to `www.ibcol.org`.

### Wrong Redirect Target

**Symptom**: The redirect points to the wrong destination URL.

**Possible Causes and Solutions**:

1. **Configuration Error**
   - **Check**: Verify the `Location` header in the `now.json` file.
   - **Solution**: Update the header to point to the correct URL and redeploy.
   - **Verification**: The `Location` header should be `https://www.ibcol.org/$1`.

2. **Cached Redirect**
   - **Check**: The browser might be caching an old redirect.
   - **Solution**: Clear browser cache or test with a different browser.
   - **Verification**: Use an incognito/private browsing window to test.

### Path Not Preserved in Redirect

**Symptom**: When visiting `ibcol.org/some-path`, the redirect goes to `www.ibcol.org` without the path.

**Possible Causes and Solutions**:

1. **Capture Group Missing**
   - **Check**: Verify that the route pattern includes a capture group.
   - **Solution**: Update the pattern to `/(.*)`  to capture the path.
   - **Verification**: The pattern should include parentheses to create a capture group.

2. **Reference Missing**
   - **Check**: Verify that the `Location` header references the capture group.
   - **Solution**: Update the header to include `$1` at the end of the URL.
   - **Verification**: The `Location` header should be `https://www.ibcol.org/$1`.

### Deployment Failures

**Symptom**: The `now` command fails to deploy the service.

**Possible Causes and Solutions**:

1. **Authentication Issue**
   - **Check**: Verify that you're logged in to the ZEIT Now CLI.
   - **Solution**: Run `now login` to authenticate.
   - **Verification**: Run `now whoami` to check your current authentication status.

2. **Team Access Issue**
   - **Check**: Verify that you have access to the `ibcol` team.
   - **Solution**: Ask a team administrator to grant you access.
   - **Verification**: Run `now teams ls` to see the teams you have access to.

3. **Configuration Error**
   - **Check**: Verify that the `now.json` file is valid JSON.
   - **Solution**: Use a JSON validator to check the syntax.
   - **Verification**: Run `cat now.json | jq` to validate the JSON (requires jq to be installed).

4. **Rate Limiting**
   - **Check**: ZEIT Now may rate limit deployments.
   - **Solution**: Wait a few minutes and try again.
   - **Verification**: Check the error message for rate limiting information.

### Wrong HTTP Status Code

**Symptom**: The redirect uses a status code other than 301.

**Possible Causes and Solutions**:

1. **Configuration Error**
   - **Check**: Verify the `status` field in the route configuration.
   - **Solution**: Set the status to `301` for a permanent redirect.
   - **Verification**: Use browser developer tools to check the HTTP status code of the redirect.

## Diagnostic Tools

### DNS Lookup

Check the current DNS configuration:

```bash
dig ibcol.org
```

### HTTP Request Inspection

Check the HTTP response headers:

```bash
curl -I http://ibcol.org
```

### ZEIT Now Deployment List

List recent deployments:

```bash
now ls --team ibcol
```

### ZEIT Now Logs

View logs for a specific deployment:

```bash
now logs [deployment-url] --team ibcol
```

## Getting Help

If you've tried the troubleshooting steps above and still can't resolve the issue:

1. **ZEIT Now Support**: Contact ZEIT Now support through their [help center](https://zeit.co/support).

2. **Team Support**: Reach out to other members of the `ibcol` team who may have experience with the service.

3. **Documentation**: Review the [ZEIT Now documentation](https://zeit.co/docs) for additional troubleshooting tips.

4. **GitHub Issues**: Check the repository's issues section for similar problems and solutions.

## Preventive Measures

To prevent issues in the future:

1. **Regular Testing**: Periodically verify that the redirect is working correctly.

2. **Monitoring**: Set up automated monitoring to alert you if the redirect stops working.

3. **Documentation**: Keep this troubleshooting guide updated with new issues and solutions.

4. **Version Control**: Make sure all changes to the configuration are tracked in version control.

