## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/apache/superset/34380)
- **PR:** [apache/superset#34380](https://github.com/apache/superset/pull/34380)
- **Issue:** N/A

# Add option to create dataset without automatically creating a chart

## Motivation

Currently, when users create a new dataset in Superset, they are automatically redirected to the chart creation page. This workflow assumes that every dataset creation should immediately be followed by chart creation. However, users often need to create datasets for organizational purposes, batch operations, or future use without immediately creating visualizations. Forcing users into the chart creation flow adds unnecessary friction and extra navigation steps when they simply want to create and save a dataset.

This enhancement will improve the user experience by providing flexibility in the dataset creation workflow, allowing users to choose whether they want to proceed to chart creation or return to the dataset list after creating a dataset.

## Current Behavior

When a user creates a dataset through the Add Dataset interface, clicking the "Create" button automatically saves the dataset and redirects them to the chart creation page. There is no option to create a dataset and return to the dataset list without going through the chart creation flow first.

**Reproduction Steps:**
1. Navigate to the Add Dataset page in Superset
2. Select a database and table to create a dataset
3. Click the "Create" button in the footer
4. Observe: You are automatically redirected to `/chart/add/?dataset={dataset_name}` (the chart creation page)
5. Observe: There is no way to create a dataset and stay on the dataset management pages without first canceling out of chart creation

## Expected Behavior

Users should have the option to choose their next action after creating a dataset. The create button should offer two distinct options:
1. Create the dataset and proceed to chart creation (current default behavior)
2. Create the dataset and return to the dataset list

The interface should clearly present both options, with the chart creation flow remaining the primary/default action to maintain backward compatibility with existing user workflows.

**Acceptance Criteria:**
- [ ] The create button is converted to a dropdown button with multiple options
- [ ] The primary button action creates the dataset and navigates to chart creation (preserving current default behavior)
- [ ] The dropdown menu includes an option to create the dataset without navigating to chart creation
- [ ] When selecting "create only", the user is redirected to the dataset list page after successful creation
- [ ] All existing tests pass and new tests cover both navigation paths
- [ ] The button remains disabled when no table is selected or when the dataset already exists

## Verification

**Manual Testing:**
1. Navigate to the Add Dataset page
2. Select a database and table
3. Verify the create button shows as a dropdown with an arrow indicator
4. Click the main button area and confirm navigation to chart creation page
5. Repeat steps 2-3, then click the dropdown arrow
6. Select "Create dataset only" from the dropdown menu
7. Verify you are redirected to `/tablemodelview/list/` (the dataset list page)
8. Confirm the dataset was created successfully in the dataset list

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx

