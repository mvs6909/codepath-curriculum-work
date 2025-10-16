# Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/BasedHardware/omi/2864)
- **PR:** [BasedHardware/omi#2864](https://github.com/BasedHardware/omi/pull/2864)
- **Difficulty:** Easy

# Bottom navigation bar should scroll to top when tapping current page icon

## Motivation

Mobile applications commonly implement a "scroll to top" behavior when users tap on the currently active navigation item. This provides a quick way for users to return to the beginning of a long list or feed without manually scrolling. This UX pattern is familiar to users from popular apps like Twitter, Instagram, and Reddit, and its absence can make the app feel less polished and harder to navigate.

Implementing this feature will improve the user experience by providing an intuitive shortcut for navigation within long content pages. Users who have scrolled deep into their conversations, memories, action items, or apps list will benefit from this quick way to return to the top of the page.

## Current Behavior

When a user is on a page in the bottom navigation bar and taps the same navigation icon again, nothing happens. The page remains at its current scroll position, forcing users to manually scroll back to the top if they want to see the beginning of the content.

**Reproduction Steps:**
1. Launch the app and navigate to the Conversations page (or any other page with scrollable content)
2. Scroll down through the list until you're several screens away from the top
3. Tap the icon in the bottom navigation bar (the same icon that's currently active)
4. Observe: The page does not scroll back to the top; it remains at the current scroll position

## Expected Behavior

When a user taps on the currently active bottom navigation icon, the page should smoothly animate to the top of its scrollable content. This should work consistently across all pages in the bottom navigation: Conversations, Action Items, Memories, and Apps.

**Acceptance Criteria:**
- [ ] Tapping the currently active bottom navigation icon triggers a smooth scroll animation to the top of the page
- [ ] The scroll animation uses an appropriate duration (around 500ms) and easing curve for a polished feel
- [ ] The scroll-to-top behavior works on all four main pages: Conversations, Action Items, Memories, and Apps
- [ ] The scroll controllers are properly disposed of to prevent memory leaks
- [ ] Tapping a different navigation icon still navigates to that page normally (existing behavior preserved)

## Verification

**Manual Testing:**
1. Navigate to the Conversations page and scroll down significantly
2. Tap the "Home" icon in the bottom navigation bar
3. Verify: The page smoothly scrolls back to the top
4. Repeat steps 1-3 for the Action Items page (tap the same icon when already on that page)
5. Repeat steps 1-3 for the Memories page
6. Repeat steps 1-3 for the Apps page
7. Verify: Tapping a differe  nt navigation icon still navigates to that page (no regression in normal navigation)
8. Test edge case: Tap the current icon when already at the top of the page - verify no errors occur

**Expected Results:**
- All pages should scroll smoothly to the top with a consistent animation
- No console errors or crashes should occur
- The animation should feel natural and not jarring
- Normal navigation between different pages should continue to work as before

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx