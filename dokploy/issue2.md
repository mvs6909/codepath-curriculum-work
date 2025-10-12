## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/Dokploy/dokploy/2250)
- **PR:** [dokploy/dokploy#2250](https://github.com/Dokploy/dokploy/pull/2250)
- **Issue:** N/A

# Add name field to user profile form

## Motivation

Users need the ability to set and update their display name in their profile settings. Currently, the profile form only allows users to update their email, password, and other settings, but there's no way to specify or change their name. This is a fundamental piece of user information that should be editable through the profile interface.

Although the underlying user schema already includes a `name` field, it is not exposed in the current profile form UI. The backend and database are capable of storing and retrieving the user's name, but the absence of this field in the form prevents users from updating it directly. Integrating the name field into the profile form will ensure that the frontend and backend remain consistent, and users can manage all relevant profile information from a single interface.

Adding a name field will improve the user experience by allowing users to personalize their profile and ensure their display name is accurate. This is especially important for multi-user environments where identifying users by name is more intuitive than by email address alone.

## Current Behavior

The user profile form does not include a field for users to enter or update their name. Users can only modify their email, password, image, and impersonation settings.

**Reproduction Steps:**
1. Log into the Dokploy application
2. Navigate to the Settings section
3. Open the Profile settings page
4. Observe the available form fields (email, password, image, allow avatar)
5. Notice there is no input field for entering or updating a user's name

## Expected Behavior

The user profile form should include an optional name field that allows users to enter and update their display name. The name should be persisted to the database when the form is submitted and should be properly validated through the form schema.

**Acceptance Criteria:**
- [ ] A `Name` input field is visible in the profile form UI
- [ ] `Name` field is displayed on the bottom left of the side-panel replacing "Accout"
- [ ] The name field is properly integrated with the form validation schema as an optional field
- [ ] The name value is correctly loaded from the user's existing profile data when the form initializes
- [ ] When the form is submitted, the name value is included in the update request
- [ ] After successful submission, the name field retains the updated value

## Verification

**Manual Testing:**
1. Navigate to the profile settings page
2. Verify that a `Name` input field is displayed in the form
3. Enter a name in the field and submit the form
4. Confirm the form submission succeeds without errors
5. Refresh the page and verify the name value persists
6. Test leaving the name field empty and verify the form still submits successfully (since it's optional)
7. Test updating the name to a different value and verify the change is saved

**Automated Testing:**
- Run the existing test suite to ensure no regressions: `npm test`
- Verify that form validation passes with and without a name value provided

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx