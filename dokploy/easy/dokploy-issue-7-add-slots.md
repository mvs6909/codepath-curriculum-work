## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/Dokploy/dokploy/2515)
- **PR:** [dokploy/dokploy#2515](https://github.com/Dokploy/dokploy/pull/2515)
- **Issue:** N/A

# Add keyboard shortcut to focus search input fields

## Motivation

Many users prefer keyboard-driven workflows and expect common keyboard shortcuts to work across applications. Currently, users must use their mouse to click into search/filter input fields throughout the application, which interrupts keyboard-focused workflows and reduces productivity.

Adding a keyboard shortcut (Cmd+K on Mac, Ctrl+K on Windows/Linux) to focus search inputs is a common UX pattern found in many modern web applications. This enhancement will improve accessibility and user experience by allowing users to quickly jump to search functionality without reaching for their mouse. While a full command palette might be added in the future, this shortcut provides immediate value for keyboard-focused users.

## Current Behavior

Search and filter input fields in the application (such as "Filter projects..." and "Filter services...") can only be focused by clicking on them with a mouse. There is no keyboard shortcut available to focus these inputs.

**Reproduction Steps:**
1. Navigate to the Projects page in the dashboard
2. Try pressing Cmd+K (Mac) or Ctrl+K (Windows/Linux) while the page is loaded
3. Observe: Nothing happens, the search input does not receive focus
4. Navigate to a specific project's environment page
5. Try the same keyboard shortcut again
6. Observe: The search input still does not receive focus

## Expected Behavior

When users press Cmd+K (Mac) or Ctrl+K (Windows/Linux), the search/filter input on the current page should receive focus, allowing them to immediately start typing their search query. The shortcut should work intelligently - it should not interfere when users are already typing in other input fields, textareas, or contenteditable elements.

**Acceptance Criteria:**
- [ ] Pressing Cmd+K (Mac) or Ctrl+K (Windows/Linux) focuses the search input on the Projects page
- [ ] Pressing Cmd+K (Mac) or Ctrl+K (Windows/Linux) focuses the search input on the Environment page
- [ ] The keyboard shortcut does not trigger when the user is already focused in an input field, textarea, select element, or contenteditable element
- [ ] The browser's default Cmd+K behavior is prevented when the shortcut is triggered
- [ ] The implementation is reusable across different pages with search inputs

## Verification

**Manual Testing:**
1. Navigate to the Projects page (`/dashboard/projects`)
2. Press Cmd+K (Mac) or Ctrl+K (Windows/Linux)
3. Verify the "Filter projects..." input receives focus and you can immediately type
4. Navigate to a project's environment page
5. Press Cmd+K (Mac) or Ctrl+K (Windows/Linux)
6. Verify the "Filter services..." input receives focus
7. Click into any other input field on the page and type some text
8. While focused in that input, press Cmd+K
9. Verify the shortcut does NOT trigger (focus stays in the current input)
10. Click outside all inputs, then press Cmd+K
11. Verify the search input receives focus again

**Expected Results:**
- The keyboard shortcut consistently focuses the appropriate search input
- No conflicts occur when users are already typing in other fields
- The feature works on both Mac (Cmd) and Windows/Linux (Ctrl)

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx