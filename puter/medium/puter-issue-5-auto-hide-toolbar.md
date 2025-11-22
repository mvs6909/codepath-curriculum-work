## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/heyputer/puter/1330)
- **PR:** [puter/puter#1330](https://github.com/heyputer/puter/pull/1330)
- **Issue:** https://github.com/HeyPuter/puter/issues/1329

# Add user preference to control toolbar auto-hide behavior

### Dependency

1. This issue is dependent on issue - https://github.com/codepath/puter/issues/8

## Motivation

The desktop toolbar currently has auto-hide functionality that automatically hides the toolbar when the user's mouse moves away from it. While this provides a cleaner interface, some users may prefer to keep the toolbar always visible for easier access to its features. Providing user control over this behavior improves the user experience by accommodating different workflow preferences.

This feature requires implementing a persistent user preference that controls whether the toolbar auto-hides, ensuring the preference is saved across sessions and properly integrated with the existing toolbar show/hide logic. This is a common pattern in desktop applications where UI behavior should be customizable and respect user choices.

## Current Behavior

The toolbar automatically hides after the user's mouse moves away from it, with no option to disable this behavior. Users who prefer a persistent toolbar have no way to prevent the auto-hide functionality.

**Reproduction Steps:**
1. Open the Puter desktop interface
2. Move your mouse to the top of the screen to reveal the toolbar
3. Move your mouse away from the toolbar area
4. Observe: The toolbar automatically hides after a short delay
5. Note: There is no setting available to disable this auto-hide behavior

## Expected Behavior

Users should be able to toggle whether the toolbar automatically hides. When auto-hide is disabled, the toolbar should remain visible at all times. This preference should persist across browser sessions.

**Acceptance Criteria:**
- [ ] A user preference for toolbar auto-hide is stored and retrieved using the key-value storage system
- [ ] When auto-hide is disabled, the toolbar remains visible and does not hide when the mouse moves away
- [ ] When auto-hide is enabled, the toolbar behaves as it currently does (auto-hiding after mouse leaves)
- [ ] The preference persists across page reloads and browser sessions
- [ ] On initial load, if auto-hide is disabled, the toolbar is shown even if it was previously hidden

## Verification

**Manual Testing:**
1. Implement the preference toggle mechanism
2. Set the preference to disable auto-hide and reload the page
3. Verify the toolbar remains visible when moving the mouse away
4. Set the preference to enable auto-hide and reload the page
5. Verify the toolbar auto-hides when moving the mouse away
6. Check browser developer tools to confirm the preference value is stored in the KV system
7. Close and reopen the browser, verify the preference persists

**Automated Testing:**
- Run existing test suite to ensure no regressions in toolbar functionality
- Verify the new preference key is properly cached in the KV module

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx