## Metadata

This issue DNE in the chatbox repo as of now. I have created it.
PR - https://github.com/mvs6909/chatbox/pull/3

# Preserve message input on session switch

## Motivation
Users lose their unsent messages when switching sessions, which is frustrating and disrupts multitasking. Preserving drafts per session is a standard feature in chat apps and would make the experience smoother and more reliable.

## Current Behavior

The message input field maintains only a single global state. When a user types a message in one session and switches to another session, the typed text is completely lost and cannot be recovered. Each session does not maintain its own draft message state.

**Reproduction Steps:**

1. Launch the chat application
  - Run 
  ```bash
  yarn install
  yarn run start
  ```
2. Create or select a chat session (Session A)
3. Begin typing a message in the input field (e.g., "This is a draft message")
4. Before sending, switch to a different session (Session B)
5. Observe that the input field is now empty
6. Switch back to Session A
7. Observe that the message you were typing is permanently lost—the input field is empty

## Expected Behavior

Each chat session should maintain its own draft message state independently. When a user types in the message input field, that draft should be associated with the current session. Switching between sessions should preserve and restore the appropriate draft message for each session, allowing users to work on multiple messages simultaneously across different conversations.

**Acceptance Criteria:**

- [ ] Each session stores and maintains its own draft message independently
- [ ] When switching from Session A to Session B, Session A's draft is preserved
- [ ] When switching back to Session A, the previously typed draft is restored to the input field
- [ ] Draft messages are persisted to storage so they survive application restarts
- [ ] When a chat session is cleared or reset, its associated draft message is also cleared
- [ ] Multiple sessions can each have their own drafts simultaneously

## Verification

**Manual Testing:**

1. Create at least three different chat sessions (A, B, and C)
2. In Session A, type "Draft message for A" but don't send it
3. Switch to Session B and type "Draft message for B" but don't send it
4. Switch to Session C and type "Draft message for C" but don't send it
5. Switch back to Session A and verify "Draft message for A" is displayed in the input
6. Switch to Session B and verify "Draft message for B" is displayed in the input
7. Switch to Session C and verify "Draft message for C" is displayed in the input
8. Restart the application and verify all drafts are still preserved
9. Clear one session's chat history and verify its draft is also cleared
10. Verify other sessions' drafts remain intact after clearing one session

**Edge Cases:**

- Test with empty drafts (switching sessions without typing)
- Test sending a message and verify the draft is cleared for that session
- Test with very long draft messages
- Test creating a new session and verify it starts with an empty draft

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx