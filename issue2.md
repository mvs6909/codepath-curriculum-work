## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/apache/superset/33694)
- **PR:** [apache/superset#33540](https://github.com/apache/superset/pull/33694)
- **Issue:** N/A

# Browser tab title not updating to chart name when opening charts

## Motivation

When users open charts in Superset, the browser tab title should reflect the name of the chart they're viewing. This improves user experience by making it easier to identify and navigate between multiple open tabs, especially when users have several charts or dashboards open simultaneously. Currently, the tab title doesn't update when viewing individual charts, making it difficult to distinguish between different chart tabs at a glance. This is a usability issue that affects how users interact with and organize their work in Superset.

Proper tab title management is also important for browser history, bookmarks, and accessibility features that rely on page titles to provide context to users.

## Current Behavior

When opening a chart in Superset's Explore view, the browser tab title does not update to reflect the chart name. The tab continues to show a generic title instead of the specific chart being viewed. Additionally, when navigating away from a dashboard, the tab title is not properly reset to a default state.

**Reproduction Steps:**
1. Navigate to Superset and open any existing chart in Explore view
2. Look at the browser tab title at the top of your browser window
3. Observe: The tab title does not display the chart name, making it difficult to identify which chart is open when multiple tabs are present
4. Open a dashboard and observe the tab title changes to the dashboard name
5. Navigate away from the dashboard

## Expected Behavior

When a user opens a chart in Explore view, the browser tab title should automatically update to display the chart's name. When the user navigates away from the chart or dashboard, the tab title should reset to a default "Superset" title. This behavior should be consistent across both the Explore view (for charts) and Dashboard view.

**Acceptance Criteria:**
- [ ] Opening a chart in Explore view updates the browser tab title to the chart's name
- [ ] The tab title updates when switching between different charts
- [ ] Navigating away from a chart resets the tab title to "Superset"
- [ ] Dashboard tab titles continue to work as expected and reset to "Superset" when navigating away
- [ ] The implementation properly cleans up and prevents memory leaks using React lifecycle patterns

## Verification

**Manual Testing:**
1. Open Superset and navigate to an existing chart in Explore view
2. Verify the browser tab title displays the chart name
3. Open a different chart in a new tab
4. Verify the new tab shows the second chart's name
5. Navigate away from the chart (e.g., go to the home page)
6. Verify the tab title resets to "Superset"
7. Open a dashboard and verify its title appears in the tab
8. Navigate away from the dashboard
9. Verify the tab title resets to "Superset"

## Automated Testing
**Automated Testing:**

To ensure the tab title updates correctly, add or update unit and integration tests in the Superset frontend codebase:

1. **Unit Tests:**  
  - Test the component responsible for setting the document title (e.g., using Jest and React Testing Library).
  - Mock chart data and verify that the tab title updates when the chart name changes.
  - Test cleanup logic to ensure the title resets to "Superset" when navigating away.
  ```bash
  cd superset-frontend
  npm run test              # Run all tests

  # To run a single test file:
  npm run test -- path/to/file.js
  ```