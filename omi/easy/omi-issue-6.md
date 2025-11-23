# Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/BasedHardware/omi/2862)
- **PR:** [BasedHardware/omi#2862](https://github.com/BasedHardware/omi/pull/2862)
- **Difficulty:** Easy

# Fix AttributeError when logging collection path during user data deletion

## Motivation

The `delete_user_data` function is responsible for cleaning up all user data from Firestore collections when a user account is deleted. Proper logging during this process is critical for debugging, auditing, and monitoring data deletion operations in production. When logging fails due to code errors, it not only breaks the deletion process but also leaves operators without visibility into what data was successfully removed.

Currently, the logging statements in this function attempt to access an attribute that doesn't exist on Firestore `CollectionReference` objects, causing the function to crash with an `AttributeError`. This prevents user data deletion from completing and creates a poor user experience when accounts cannot be properly removed from the system.

## Current Behavior

The `delete_user_data` function in `backend/database/users.py` attempts to log the collection path when deleting documents from Firestore collections. However, the code tries to access `collection_ref.path` directly, which causes a runtime error because `CollectionReference` objects do not have a `path` attribute.

**Reproduction Steps:**

1. Navigate to the `backend/database/users.py` file
2. Locate the `delete_user_data(uid: str)` function
3. Examine the logging statements that reference `collection_ref.path`
4. Run the function with a valid user ID (or simulate the execution path)
5. Observe: The function raises `AttributeError: 'CollectionReference' object has no attribute 'path'` when it attempts to execute the print statements
6. Expected: The function should log collection paths without errors
7. Actual: The function crashes and user data deletion fails

## Expected Behavior

The logging statements should successfully output the collection path without raising any errors. The collection path should be constructed using available attributes from the `CollectionReference` object, allowing the deletion process to complete while providing clear visibility into which collections are being processed.

**Acceptance Criteria:**

- [ ] The function no longer raises `AttributeError` when attempting to log collection paths
- [ ] Log messages correctly display the full collection path in the format expected for Firestore paths
- [ ] The collection path is constructed using attributes that actually exist on `CollectionReference` objects
- [ ] All logging statements in the batch deletion loop work correctly
- [ ] The user data deletion process completes successfully with proper logging output

## Verification

**Manual Testing:**
1. Run the `delete_user_data` function with a test user ID that has data in multiple collections
2. Monitor the console output for log messages about document deletion
3. Verify that no `AttributeError` exceptions are raised
4. Confirm that collection paths are displayed correctly in the log output
5. Ensure the function completes successfully and all user data is deleted

**Code Inspection:**
1. Review the Firestore Python SDK documentation for `CollectionReference` to identify which attributes are available for constructing paths
2. Verify that the logging statements use only valid attributes
3. Confirm that the constructed path matches the expected Firestore collection path format

**Success Indicators:**
- The function executes without AttributeError exceptions
- Log messages display meaningful collection path information
- User data deletion completes successfully with full traceability

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx