# Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/BasedHardware/omi/2900)
- **PR:** [BasedHardware/omi#2900](https://github.com/BasedHardware/omi/pull/2900)
- **Difficulty:** Medium

Note: Requires changes in the backend

# Add user-configurable conversation timeout duration

## Motivation

Currently, conversations in the app automatically end after a hardcoded 2-minute silence period. This one-size-fits-all approach doesn't accommodate different use cases. Users attending long lectures or meetings may find their conversations prematurely ended during natural pauses, while others in quick interactions might prefer shorter timeouts. Additionally, some users want complete manual control over when conversations start and end.

Providing configurable timeout options empowers users to tailor the app's behavior to their specific needs, whether they're recording a brief chat, a lengthy class, or an all-day event. This flexibility improves user experience and makes the app more versatile across different recording scenarios.

## Current Behavior

The conversation timeout duration is hardcoded to 120 seconds (2 minutes) throughout the application. When users are recording a conversation, if there is no speech detected for 2 minutes, the conversation is automatically ended and summarized. Users have no ability to change this behavior.

**Reproduction Steps:**
1. Navigate to the app's settings/profile page
2. Look for any option to configure conversation timeout duration
3. Observe: No such setting exists
4. Start a conversation recording
5. Let the recording sit in silence for 2 minutes
6. Observe: The conversation automatically ends after exactly 2 minutes, with no way to customize this duration
7. Check the UI text during recording
8. Observe: The text "Conversation is summarized after 2 minutes of no speech 🤫" is hardcoded and always displays 2 minutes

## Expected Behavior

Users should be able to configure how long the app waits in silence before automatically ending a conversation. The setting should be accessible from the profile/settings page and should offer multiple timeout options to accommodate different use cases.

**Acceptance Criteria:**
- [ ] A new "Conversation Timeout" setting is available in the profile/settings page with an appropriate icon and description
- [ ] Tapping the setting opens a dialog with timeout options: 2 minutes, 5 minutes, 10 minutes, 30 minutes, and "Never" (manual end only)
- [ ] The selected timeout preference is persisted and survives app restarts
- [ ] The conversation recording page displays dynamic text reflecting the currently selected timeout duration (e.g., "Conversation is summarized after 5 minutes of no speech 🤫" or "Conversation will only end manually 🤫")
- [ ] The confirmation dialog when manually ending a conversation also displays the correct timeout setting in its hint text
- [ ] The backend respects the timeout setting sent from the client and uses it to determine when to automatically end conversations
- [ ] When "Never" is selected, conversations only end when manually stopped by the user

## Verification

**Manual Testing:**
1. Open the app and navigate to the profile/settings page
2. Verify a "Conversation Timeout" option is present
3. Tap the option and verify a dialog appears with 5 timeout choices (2, 5, 10, 30 minutes, and Never)
4. Select "5 minutes" and close the dialog
5. Start a conversation recording
6. Verify the UI text shows "Conversation is summarized after 5 minutes of no speech 🤫"
7. Tap the stop button to manually end the conversation
8. Verify the confirmation dialog shows "Conversation is summarized after 5 minutes of no speech."
9. Restart the app and verify the setting persists
10. Change the setting to "Never"
11. Start a new conversation and verify the text shows "Conversation will only end manually 🤫"
12. Test that a conversation with "Never" selected does not auto-end after extended silence
13. Test each timeout option (2, 10, 30 minutes) to ensure the UI updates correctly

**Backend Verification:**
1. Monitor WebSocket connection parameters to ensure the `conversation_timeout` parameter is being sent with the correct value
2. Verify that conversations respect the timeout duration set by the user
3. Test that the "Never" option (-1 value) prevents automatic conversation ending

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx