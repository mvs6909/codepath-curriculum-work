## Metadata (not to include in the student issue)

- PR Link: https://github.com/scalar/scalar/pull/6469/files
- Issue link: NA
- Tool: https://openbootstrap.onrender.com/pr/scalar/scalar/6469

# Add configuration option to display operation path instead of summary in sidebar

## Motivation

API documentation tools typically display operation summaries in the sidebar navigation to help users understand what each endpoint does. However, when working with APIs that have verbose or lengthy operation summaries, the sidebar becomes difficult to scan and navigate. Long summary text makes it harder to quickly find the endpoint you're looking for, and the sidebar becomes unnecessarily tall and wordy.

Providing a configuration option to display the operation path (e.g., `/planets/{planetId}`) instead of the summary (e.g., "Get a particular celestial body from the solar system currently classified as a planet") would give users control over sidebar presentation. This improves usability for APIs with verbose documentation while maintaining backward compatibility for those who prefer summary-based navigation.

## Current Behavior

The API reference sidebar always displays the operation summary for each endpoint. There is no way to configure this behavior to show the operation path instead.

**Reproduction Steps:**

1. Load an OpenAPI specification with verbose operation summaries (e.g., modify the Galaxy API example to have a summary like "Get a particular celestial body from the solar system currently classified as a planet" for the `/planets/{planetId}` endpoint)
2. Open the API reference documentation and observe the sidebar
3. Observe: The sidebar displays the full operation summary text, making it lengthy and difficult to scan
4. Attempt to find a configuration option to display paths instead of summaries
5. Observe: No such configuration option exists

## Expected Behavior

Users should be able to configure whether the sidebar displays operation summaries or operation paths. When configured to use paths, the sidebar should show the endpoint path (e.g., `/planets/{planetId}`) instead of the summary text, making it more concise and easier to scan.

**Acceptance Criteria:**

- [ ] A new configuration option is available that controls whether the sidebar displays operation summaries or paths
- [ ] The default behavior remains unchanged (displays summaries) to maintain backward compatibility
- [ ] When configured to use paths, the sidebar displays the operation path for each endpoint
- [ ] The configuration option is properly typed and validated across all integration layers
- [ ] Tests verify both the default behavior and the new path-based display option
- [ ] Documentation clearly explains the new configuration option and when to use it

## Verification

**Manual Testing:**
1. Configure the API reference to use the default setting (or omit the configuration)
2. Load an OpenAPI specification and verify the sidebar displays operation summaries
3. Configure the API reference to display operation paths
4. Reload the same OpenAPI specification and verify the sidebar now displays operation paths instead of summaries
5. Test with an API that has very long operation summaries to confirm the sidebar is more concise when using paths

