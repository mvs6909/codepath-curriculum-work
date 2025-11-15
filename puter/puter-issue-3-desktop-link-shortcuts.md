## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/heyputer/puter/1219)
- **PR:** [puter/puter#1219](https://github.com/heyputer/puter/pull/1219)
- **Issue:** https://github.com/HeyPuter/puter/issues/682


# Add desktop link shortcuts for quick access to URLs

## Motivation

Users need a convenient way to create shortcuts to frequently visited websites directly on their desktop, similar to how desktop operating systems allow creating web shortcuts. Currently, there's no native way to save and organize web links within the Puter desktop environment. This feature would improve user productivity by allowing quick access to important web resources without needing to remember or type URLs repeatedly.

Adding link shortcuts enhances the desktop experience by treating URLs as first-class items in the file system, complete with appropriate icons and intuitive interaction patterns. This bridges the gap between the Puter environment and external web resources, making the desktop feel more integrated and functional.

## Current Behavior

The desktop environment lacks any mechanism for creating, storing, or opening web link shortcuts. Users cannot save URLs for quick access, and there's no context menu option to create links. Additionally, pasting a URL onto the desktop has no special handling—it would either fail or create a plain text file.

**Reproduction Steps:**
1. Right-click on an empty area of the desktop
2. Observe the "New" submenu options (currently only shows options like Folder, Text File, HTML File, etc.)
3. Try to paste a URL (e.g., `https://example.com`) onto the desktop
4. Observe: There is no "New Link" option, and pasting URLs doesn't create any special shortcut file
5. Note: There's no way to store or organize web links within the desktop environment

## Expected Behavior

Users should be able to create web link shortcuts that behave like native desktop items. Right-clicking the desktop should offer a "New Link" option that prompts for a URL. Pasting a URL should automatically create a link shortcut file. These shortcuts should display with a recognizable link icon and open the URL in a new browser tab when double-clicked.

**Acceptance Criteria:**
- [ ] Right-clicking empty desktop space shows a "New Link" option in the context menu
- [ ] Selecting "New Link" prompts the user for a URL with validation (must start with http:// or https://)
- [ ] Successfully entering a URL creates a `.weblink` file with an appropriate name derived from the domain
- [ ] Pasting a URL onto the desktop automatically creates a `.weblink` shortcut file
- [ ] Link shortcut files display with a link icon (not a generic file icon)
- [ ] Double-clicking a link shortcut opens the URL in a new browser tab
- [ ] The link shortcut stores the URL persistently and can be reopened after page refresh

## Verification

**Manual Testing:**
1. Right-click on an empty area of the desktop and verify "New Link" appears in the context menu
2. Click "New Link" and enter a valid URL (e.g., `https://github.com`)
3. Verify a new file is created with a link icon and appropriate name
4. Double-click the created link shortcut and verify it opens the correct URL in a new tab
5. Copy a URL from your browser's address bar (e.g., `https://www.google.com`)
6. Paste it onto the desktop (Ctrl+V or Cmd+V)
7. Verify a link shortcut is automatically created
8. Refresh the page and double-click the link again to verify persistence
9. Try entering an invalid URL (without http:// or https://) and verify validation prevents creation

**Expected Results:**
- All link shortcuts should display with a consistent link icon
- URLs should open in new tabs with proper security attributes (noopener, noreferrer)
- Link files should have the `.weblink` extension
- File names should be intelligently derived from the URL domain
- The system should handle errors gracefully with user-friendly messages

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx