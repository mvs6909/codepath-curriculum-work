# Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/gitlab-org/gitlab/206407)
- **PR:** [gitlab-org/gitlab#206407](https://gitlab.com/gitlab-org/gitlab/-/merge_requests/206407)
- **Issue:** N/A

# Add alerts for passkeys in password page

## Motivation

Users who manage their account security need clear visibility into their authentication methods. Currently, the password settings page doesn't provide any information about passkeys, which are an important modern authentication mechanism. This creates a gap in user awareness—users may not know whether they have passkeys configured or understand how passkeys relate to their password security.

By adding contextual alerts to the password page, we can inform users about their passkey status and guide them toward better security practices. This improves the overall user experience by making authentication options more discoverable and helping users understand the relationship between passwords and passkeys.

## Current Behavior

The password settings page does not display any information about the user's passkey configuration status. Users visiting this page have no visibility into whether they have passkeys set up or any guidance on how passkeys might enhance their account security.

**Reproduction Steps:**
1. Enable the `passkeys` feature flag at `https://gdk.test:3443/rails/features/passkeys`
2. Navigate to `https://gdk.test:3443/-/user_settings/password`
3. Observe: The page displays password settings but no alerts or information about passkeys
4. Check the page with different user configurations (with and without existing authentication methods)
5. Observe: No differentiation in messaging based on user's passkey status

## Expected Behavior

The password settings page should display contextual alerts that inform users about their passkey status. The alert content and messaging should differ based on whether the user has passkeys configured:

- Users **without** passkeys should see an alert encouraging them to set up passkeys, with appropriate links to documentation and the passkey configuration page
- Users **with** passkeys should see an alert acknowledging their existing passkeys, with relevant links for management

Additionally, any unnecessary UI elements (such as a search bar) should be removed from the page to improve clarity and focus.

**Acceptance Criteria:**
- [ ] An alert is displayed when the user has no passkeys configured
- [ ] A different alert is displayed when the user has at least one passkey configured
- [ ] Both alerts include appropriate links (to documentation and passkey management page)
- [ ] The alerts only appear when the `passkeys` feature flag is enabled
- [ ] The page layout is clean and focused on relevant settings

## Verification

**Manual Testing:**
1. Enable the `passkeys` feature flag at `https://gdk.test:3443/rails/features/passkeys`
2. Navigate to `https://gdk.test:3443/-/user_settings/password` with a user account that has no passkeys
3. Verify that an appropriate alert is displayed for users without passkeys
4. Configure the user account to have at least one passkey (or test with a different user state)
5. Return to the password settings page
6. Verify that a different alert is displayed for users with passkeys
7. Confirm that both alerts contain relevant links and appropriate messaging
8. Disable the `passkeys` feature flag and verify the alerts do not appear

**Expected Results:**
- Two distinct alert states are visible depending on passkey configuration
- Alerts provide clear, helpful information to users
- The UI is clean and professional
- Feature flag properly controls alert visibility