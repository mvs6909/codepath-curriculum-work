## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/apache/superset/33656)
- **PR:** [apache/superset#33540](https://github.com/apache/superset/pull/33656)
- **Issue:** N/A
- Notes: Took multiple attempts with Cursor to get the implementation right. The issue was primarily focused for All Records query not being setup correctly. Interactive session was helpful to figure out the root cause.

# Add configurable percentage metric calculation mode for Table charts

## Motivation

When displaying percentage metrics in Table charts with row limits, users currently face a critical limitation: percentages are always calculated based only on the limited rows returned by the query, not the entire dataset. For example, if a user queries the top 10 products by sales and wants to see each product's percentage contribution to total sales, the current implementation calculates percentages based only on those 10 products, not all products in the database. This produces misleading results—the top 10 products might show percentages summing to 100%, when in reality they might only represent 30% of total sales.

This feature will allow users to choose between two calculation modes: calculating percentages based on the limited result set (current behavior, useful for relative comparisons within the visible data), or calculating percentages based on the full dataset (useful for understanding true contribution to the overall total). The latter mode requires fetching totals from the entire dataset through an auxiliary query, then using those totals in post-processing calculations.

## Current Behavior

Table charts with percentage metrics and row limits calculate percentages using only the rows returned by the query, which can misrepresent the data's true contribution to the overall total.

**Reproduction Steps:**
1. Go to the Explore view and create a new `Table` chart.
2. Set the chart to aggregate mode and select a groupby column (e.g., `birth_names`).
3. Add a regular metric (e.g., "sum of names") and a percentage metric for the same measure.
4. Set a row limit (e.g., 10) to display only the top results.
5. Run the query and review the percentage column.
6. Change the row limit (e.g., to 5) and re-run the query.
7. Observe: The percentages always sum to 100% across the visible rows, even though these rows may only represent a portion of the total dataset.
8. Expected: There is currently no way to see what percentage these top rows contribute to the entire dataset.

## Expected Behavior

Users should be able to choose how percentage metrics are calculated through a new control in the Table chart configuration panel. When "All records" mode is selected, percentages should reflect contribution to the entire dataset total, not just the visible rows.

**Acceptance Criteria:**
- [ ] A new "Percentage metric calculation" control dropdown appears in the `Table` chart control panel when in aggregate mode with percentage metrics configured
- [ ] The control offers two options: "Row limit" (default, current behavior) and "All records" (new behavior)
- [ ] When "Row limit" is selected, percentage calculations use only the rows returned by the main query
- [ ] When "All records" is selected, an auxiliary query fetches totals from the full dataset, and these totals are used for percentage calculations
- [ ] Switching between modes updates the chart to reflect the different calculation methods
- [ ] Unit tests are added to test the new functionality
- [ ] The feature works correctly in combination with other Table chart features like show_totals and time comparisons

## Verification

**Manual Testing:**
1. Observe the table chart in aggregate mode with a groupby column and both regular and percentage metrics
2. Set a row limit (e.g., 10) that returns a subset of available data
3. Verify the new "Percentage metric calculation" control appears in the control panel
4. With "Row limit" selected, confirm percentages sum to approximately 100% across visible rows
5. Switch to "All records" mode and re-run the query
6. Verify percentages now reflect contribution to the full dataset (should sum to less than 100% if row limit excludes data)
7. Test with different row limits and verify calculations remain accurate
8. Test in combination with the "Show totals" feature to ensure compatibility

**Automated Testing:**
- Run tests - https://superset.apache.org/docs/contributing/howtos#testing
- Verify new tests pass that cover both calculation modes
- Run Python backend tests to verify query processing logic
- Confirm post-processing contribution calculations handle the new totals parameter correctly

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx