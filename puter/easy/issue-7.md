## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/heyputer/puter/1589)
- **PR:** [puter/puter#1589](https://github.com/heyputer/puter/pull/1589)
- **Issue:** https://github.com/HeyPuter/puter/issues/1662

# Window incorrectly focuses when close or minimize button is clicked

## Motivation

When users interact with a windowed interface, they expect intuitive behavior that doesn't disrupt their workflow. Currently, clicking the close or minimize button on a window causes that window to be brought to the foreground before the action is executed. This creates a jarring user experience, especially when trying to close or minimize a window that is partially obscured by another window. Instead of simply closing the background window, it first jumps to the front, disrupting the user's visual context and making the interface feel unresponsive to their intent.

This behavior violates user expectations for window management. When a user clicks a close or minimize button, their intent is clear: they want to dismiss or hide that specific window without changing the focus or z-order of their workspace. Fixing this will make the interface feel more polished and responsive to user intent.

## Current Behavior

When a user clicks the close (X) or minimize button on any window, the window is first brought to the foreground (focused/activated) before the close or minimize action is executed. This happens even when the window is behind other windows.

![Video demo](../assets/issue-7.mov)

**Reproduction Steps:**
1. Open two or more windows in the application, ensuring they overlap
2. Position the windows so that one window partially covers another
3. Click the close button (X) or minimize button on the window that is behind/underneath
4. Observe: The background window jumps to the foreground first, then closes or minimizes
5. Expected: The window should close or minimize without being brought to the foreground

## Expected Behavior

When a user clicks the close or minimize button on a window, the window should execute the requested action (close or minimize) without first being brought to the foreground or changing focus. The current window focus and z-order should remain unchanged until after the action completes.

**Acceptance Criteria:**
- [ ] Clicking the close button on a background window closes it without bringing it to the foreground first
- [ ] Clicking the minimize button on a background window minimizes it without bringing it to the foreground first
- [ ] Clicking other parts of a window (title bar, content area) still brings the window to the foreground as expected
- [ ] The fix works consistently across different window states and configurations
- [ ] No regression in other window management behaviors

## Verification

**Manual Testing:**
1. Open 3-4 windows in the application with overlapping positions
2. Identify a window that is partially or fully behind other windows
3. Click the close button on the background window and verify it closes without coming to the foreground
4. Repeat with the minimize button on a different background window
5. Click on the title bar or content area of a background window and verify it still comes to the foreground as expected
6. Test with windows in various states (maximized, minimized, different sizes)

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx