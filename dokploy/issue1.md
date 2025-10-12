## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/Dokploy/dokploy/2236)
- **PR:** [dokploy/dokploy#2236](https://github.com/Dokploy/dokploy/pull/2236)
- **Issue:** N/A

# Password displayed in plain text in application security view

## Motivation

Security best practices dictate that passwords should never be displayed in plain text in user interfaces. This prevents shoulder surfing attacks and accidental exposure when users share screenshots or during screen sharing sessions. Currently, the application security view displays basic authentication passwords in plain text, which poses a security risk.

The application already has established patterns for handling password visibility in other areas of the interface. Maintaining consistency across the application improves user experience and ensures security best practices are applied uniformly throughout the codebase.

## Current Behavior

In the Application Advanced Security view, basic authentication credentials are displayed with both username and password visible in plain text. The password field shows the actual password value without any masking or toggle visibility option, making it vulnerable to exposure.

**Reproduction Steps:**
1. Start Dokploy locally by running `pnpm run dokploy:dev`
1. Access an application from the dashboard. If none exists, create a mock Project and Application first.
2. Select the `Advanced > Security` tab under Applications.
4. Observe the basic authentication credentials display
5. Notice that the password is displayed in plain text and fully visible

## Expected Behavior

The password field in the security view should be masked by default, with an option to toggle visibility when needed. This should follow the same pattern used elsewhere in the application for password fields.

**Acceptance Criteria:**
- [ ] Password field in the "Add Security" form uses a password input type to mask the password during entry
- [ ] Password field in the "Show Security" display view uses a toggle visibility component to hide the password by default
- [ ] Users can toggle password visibility on/off in the display view when needed
- [ ] The implementation is consistent with password field patterns used elsewhere in the application
- [ ] The layout and styling remain consistent with the existing design

## Verification

**Manual Testing:**
1. Navigate to Application > Advanced > Security
2. Add a new basic authentication credential with a username and password
3. Verify that during entry, the password field masks the input
4. After saving, verify the password is hidden by default in the credentials list
5. Verify there is a toggle button to show/hide the password
6. Click the toggle and verify the password becomes visible
7. Click the toggle again and verify the password is hidden again
8. Compare the password field behavior with other password fields in the application (e.g., database credentials, environment variables) to ensure consistency

**Expected Results:**
- Password input is masked during entry
- Saved passwords are hidden by default in the display view
- Toggle functionality works correctly to show/hide passwords
- UI remains clean and consistent with the rest of the application