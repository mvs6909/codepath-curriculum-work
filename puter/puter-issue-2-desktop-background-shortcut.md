## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/heyputer/puter/1635)
- **PR:** [puter/puter#1635](https://github.com/heyputer/puter/pull/1635)
- **Issue:** https://github.com/HeyPuter/puter/issues/1608

# Add 'Set as Desktop Background' context menu item to images

## Motivation

Users currently have no quick way to set an image as their desktop background directly from the file system. They must navigate through settings or other indirect methods to change their wallpaper. Adding a context menu option when right-clicking on image files would provide a convenient, intuitive way for users to personalize their desktop environment.

This feature enhances the user experience by reducing friction in a common personalization task. It follows familiar desktop environment patterns where users expect to be able to right-click images and set them as backgrounds, making the interface more intuitive and user-friendly.

## Current Behavior

When users right-click on image files in the file system, they see various context menu options like Open, Download, Copy, etc., but there is no option to set the image as their desktop background.

**Reproduction Steps:**
1. Navigate to any folder containing image files (PNG, JPG, etc.)
2. Right-click on an image file
3. Observe the context menu that appears
4. Observe: No "Set as Desktop Background" option is present
5. Issue: Users must use alternative methods to change their desktop background

## Expected Behavior

When users right-click on image files, they should see a "Set as Desktop Background" option in the context menu. Clicking this option should immediately apply the image as the desktop background and persist this preference to their user settings.

**Acceptance Criteria:**
- [ ] A "Set as Desktop Background" menu item appears in the context menu when right-clicking on image files
- [ ] The menu item only appears for image file types (not for folders, text files, or other non-image files)
- [ ] Clicking the menu item immediately updates the desktop background to the selected image
- [ ] The desktop background preference is saved to the user's account and persists across sessions
- [ ] The menu item does not appear for items in the trash or for trashed items

## Verification

**Manual Testing:**
1. Right-click on various image files (PNG, JPG, GIF, etc.) and verify the "Set as Desktop Background" option appears
2. Click the option and confirm the desktop background changes immediately
3. Right-click on non-image files (text files, folders, etc.) and verify the option does NOT appear
4. Set an image as background, refresh the page or log out/in, and verify the background persists
5. Right-click on images in the trash folder and verify the option does not appear

**Automated Testing:**
- Run existing test suites to ensure no regressions were introduced
- Verify that the internationalization system properly displays the menu item text

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx