## Metadata

This issue DNE in the chatbox repo as of now. I have created it.
PR - https://github.com/mvs6909/chatbox/pull/2

# Add confirmation dialog for deleting sessions

## Motivation

Accidentally deleting a chat session means losing all its messages forever. Right now, there's no warning—one click and it's gone. Adding a confirmation dialog helps prevent mistakes and protects important conversations.

## Current Behavior

When a user clicks the delete button on a session, the session is immediately and permanently deleted without any confirmation or warning. There is no opportunity for the user to cancel the action if it was accidental.

**Reproduction Steps:**

1. Launch the chat application
  - Run 
  ```bash
  yarn install
  yarn run start
  ```
  - Note: You might have a warning for `sass-loader`, press `X` for now.
2. Create a new chat session and send some messages
3. Locate the delete button for the session in the session list
4. Click the delete button once
5. Observe that the session is immediately deleted without any confirmation dialog
6. Note that there is no way to undo or recover the deleted session
7. Recognize the risk: an accidental click results in permanent data loss

## Expected Behavior

When a user clicks the delete button on a session, a confirmation dialog should appear asking the user to confirm the deletion. The dialog should clearly identify which session is being deleted and provide both "Cancel" and "Delete" options. Only when the user explicitly confirms by clicking "Delete" in the dialog should the session actually be removed.

**Acceptance Criteria:**

- [ ] Clicking the delete button opens a confirmation dialog instead of immediately deleting the session
- [ ] The dialog displays the name or identifier of the session being deleted
- [ ] The dialog has a "Cancel" button that closes the dialog without deleting anything
- [ ] The dialog has a "Delete" button that confirms the deletion and removes the session
- [ ] The dialog follows the same design pattern and styling as other dialogs in the application
- [ ] Sessions are only deleted when the user explicitly confirms through the dialog

## Verification

**Manual Testing:**

1. Launch the application and create multiple chat sessions with different names
2. Click the delete button on one of the sessions
3. Verify that a confirmation dialog appears instead of immediate deletion
4. Check that the dialog shows the correct session name/identifier
5. Click "Cancel" and verify the dialog closes and the session remains intact
6. Click delete again, then click "Delete" in the dialog and verify the session is removed
7. Test with multiple sessions to ensure the correct session is always identified in the dialog
8. Verify the dialog styling is consistent with other dialogs in the application

**Edge Cases:**

- Test clicking outside the dialog or pressing ESC to ensure proper cancel behavior
- Verify that deleting the last session works correctly
- Test rapid clicks to ensure the dialog state is managed properly

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx