## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/Dokploy/dokploy/2507)
- **PR:** [dokploy/dokploy#2507](https://github.com/Dokploy/dokploy/pull/2507)
- **Issue:** https://github.com/Dokploy/dokploy/issues/1485
- **Difficulty**: Medium

# Add custom title/description support for API/CLI deployments

## Motivation

Currently, all deployments triggered via the API or CLI are labeled with generic titles like "Manual deployment" or "Rebuild deployment," making it impossible to distinguish between different deployments in the deployment history. Only webhook-triggered deployments benefit from descriptive labels (using commit messages), which creates an inconsistent experience and makes it difficult to track and audit specific deployments.

This limitation affects teams using the API or CLI for automated deployments, as they cannot provide context about what each deployment contains (e.g., "Hotfix: Fix login bug" or "Feature: Add user authentication"). Adding support for custom titles and descriptions will improve deployment tracking, make audit logs more meaningful, and provide consistency across all deployment methods.

## Current Behavior

When deployments are triggered via the API or CLI, they appear in the deployment history with hardcoded, generic titles that provide no context about the deployment's purpose or contents. This makes it challenging to identify specific deployments when reviewing history or troubleshooting issues.

**Reproduction Steps:**

1. **Setup Application**
   1. Spin up Dokploy locally at `localhost:3000` using `pnpm run dokploy:dev`
   2. Create a Project
   3. Add `Service > Application` and name it `busybox-app`
   4. Configure the application:
      - Provider: `Docker`
      - Docker Image: `busybox`
      - Click Save (leave other fields empty)
   5. Click "Deploy" and verify deployment succeeds
   6. Copy the Application ID from the URL: `http://localhost:3000/dashboard/project/<<PROJECT_ID>>/services/application/<<APPLICATION_ID>>`

2. **Create API Key**
   1. Go to `Dashboard > Settings > Profile`
   2. Create and copy an API Key
   3. Tip: Explore "Swagger API" from the API Key page to explore all available APIs

3. **Trigger Deployment via API**
```bash
   curl -X 'POST' \
     'http://localhost:3000/api/application.deploy' \
     -H 'accept: application/json' \
     -H 'Content-Type: application/json' \
     -H 'x-api-key: <<YOUR_API_KEY>>' \
     -d '{"applicationId": "<<APPLICATION_ID>>"}'
```
4. **Observe Issues**
  - Navigate to deployment history in the UI
  - Deployment shows generic title "Manual deployment" with no description
  - Trigger a rebuild using application.redeploy → shows "Rebuild deployment" with no description
  - Compare with webhook-triggered deployments (which show commit messages)
  - Result: No way to distinguish between different API/CLI deployments or provide custom context

## Expected Behavior

Users should be able to optionally specify custom titles and descriptions when triggering deployments via the API or CLI. These custom values should appear in the deployment history, similar to how commit messages are displayed for webhook-triggered deployments. When custom values are not provided, the system should maintain backward compatibility by using the existing default titles.

**Acceptance Criteria:**
- [ ] TRPC `application.deploy` and `application.redeploy` endpoints accept optional `title` and `description` parameters
- [ ] TRPC `compose.deploy` and `compose.redeploy` endpoints accept optional `title` and `description` parameters
- [ ] External API `/deploy` endpoint accepts optional `title` and `description` parameters for all application types
- [ ] When custom title/description are provided, they appear in the deployment history UI
- [ ] When custom title/description are not provided, deployments use existing default titles ("Manual deployment", "Rebuild deployment", etc.) maintaining backward compatibility
- [ ] Schema validation properly handles optional parameters and rejects invalid inputs
- [ ] All existing deployments continue to work without modification

## Verification

**Manual Testing:**
1. Test TRPC endpoints with custom title and description parameters and verify they appear in the deployment history UI
2. Test TRPC endpoints without custom parameters and verify default titles are used
3. Test external API endpoint with custom `title` and `description` parameters
4. Test external API endpoint without custom parameters and verify backward compatibility
5. Verify schema validation rejects invalid inputs (e.g., wrong data types)
6. Confirm the deployment history UI displays custom titles and descriptions correctly

**Automated Testing:**
- Verify schema validation tests pass for the new optional parameters
- Confirm all deployment types (application, compose, preview) handle the new parameters correctly

<details>
  <summary><strong>Implementation Hints (click to expand)</strong></summary>

  - Add optional `title` and `description` fields to deployment and compose schemas
  - Update API endpoints (TRPC and external) to accept these optional parameters
  - Maintain backward compatibility by defaulting to current generic titles when omitted

</details>

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx