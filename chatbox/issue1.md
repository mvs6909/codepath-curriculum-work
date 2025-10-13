## Metadata

This issue DNE in the chatbox repo as of now. I have created it.
PR - https://github.com/mvs6909/chatbox/pull/1

# Add keyboard shortcut to create new chat

## Motivation

Keyboard shortcuts are essential for power users who want to navigate applications efficiently without relying on mouse interactions. In a chat application where users frequently start new conversations, the ability to quickly create a new chat session using a keyboard shortcut significantly improves productivity and user experience. This is a common pattern in modern applications (browsers use Cmd/Ctrl+N for new windows/tabs, text editors for new files, etc.), and users expect this functionality.

Currently, users must locate and click the "New Chat" button with their mouse to start a new conversation. This interrupts their workflow, especially when they're typing or navigating with the keyboard. Adding a keyboard shortcut will make the application feel more polished and professional, and will align with user expectations from similar applications. The UI already hints at this functionality with a placeholder comment, indicating this feature was planned but not yet implemented.

## Current Behavior

The application only supports creating a new chat session through mouse interaction with the "New Chat" button. There is no keyboard shortcut implemented, even though the UI contains a comment suggesting that Cmd+N / Ctrl+N should be supported.

**Reproduction Steps:**

1. Launch the chat application
  - Run 
  ```bash
  yarn install
  yarn run start
  ```
  - Note: You might have a warning for `sass-loader`, press `X` for now.
2. Try pressing `Cmd+N` (on macOS) or `Ctrl+N` (on Windows/Linux)
3. Observe that nothing happens - no new chat is created
4. Click the "New Chat" button with your mouse
5. Observe that a new chat session is successfully created
6. Note the discrepancy: mouse interaction works, but keyboard shortcut does not

## Expected Behavior

Users should be able to create a new chat session by pressing `Cmd+N` on macOS or `Ctrl+N` on Windows/Linux from anywhere within the application. The keyboard shortcut should trigger the same functionality as clicking the "New Chat" button, providing a seamless keyboard-driven workflow.

**Acceptance Criteria:**

- [ ] Pressing `Cmd+N` on macOS creates a new chat session
- [ ] Pressing `Ctrl+N` on Windows/Linux creates a new chat session
- [ ] The cursor auto-switches to the new chat prompt
- [ ] The keyboard shortcut works regardless of which part of the UI currently has focus
- [ ] The UI displays the keyboard shortcut hint next to the "New Chat" button (updating the existing placeholder)
- [ ] The keyboard shortcut does not interfere with existing browser/system shortcuts or cause unexpected behavior

## Verification

**Manual Testing:**

1. Run the application on macOS and press `Cmd+N` - verify a new chat is created
2. Run the application on Windows/Linux and press `Ctrl+N` - verify a new chat is created
3. Test the shortcut from different states: while typing in the chat input, while viewing the chat history, after selecting text, etc.
4. Verify the UI now shows the keyboard shortcut hint (e.g., "⌘N" or "Ctrl+N") near the "New Chat" button
5. Confirm the "New Chat" button still works when clicked with the mouse
6. Test that the shortcut doesn't conflict with browser defaults (e.g., opening a new browser window)

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx