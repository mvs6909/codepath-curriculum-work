# Add option to automatically set window title to opened file's name

## Motivation

When users open multiple files of the same type (e.g., multiple PDF documents or spreadsheets) in Puter, all windows display the same generic application name as their title. This makes it difficult to distinguish between windows and navigate between different open files, creating a poor user experience when working with multiple documents simultaneously.

Application developers should have the ability to opt-in to automatically setting their app's window title to the opened file's name. This would help users identify which file is open in which window, improving multitasking and window management. This feature should be configurable through the app settings in the developer center, giving developers control over their app's window behavior.

## Current Behavior

When multiple files are opened in the same application, all windows display the same title (the application name). There is no way for developers to configure their apps to automatically use the opened file's name as the window title.

**Reproduction Steps:**
1. Navigate to the Puter desktop
2. Create or locate two different PDF files (e.g., "document1.pdf" and "document2.pdf")
3. Open both PDF files in the default PDF viewer application
4. Observe: Both windows show the same title (the application name), making it impossible to distinguish which window contains which document without clicking into each window
5. Navigate to the developer center and edit any application
6. Observe: There is no setting available to control window title behavior for opened files

## Expected Behavior

Developers should be able to enable a setting in the app edit section that automatically sets the window title to the opened file's name when users open files in their application. When this setting is enabled and a user opens a file, the window title should display the file's basename instead of just the application name.

**Acceptance Criteria:**
- [ ] A new checkbox option is available in the app edit section labeled "Automatically set window title to opened file's name"
- [ ] The checkbox state is properly saved when updating an app's settings
- [ ] The checkbox state is correctly loaded and displayed when editing an existing app
- [ ] When the setting is enabled for an app, opening a file in that app sets the window title to the file's name
- [ ] The form change tracking system properly detects changes to this new checkbox
- [ ] The reset functionality properly restores the original checkbox state when discarding changes

## Verification

**Manual Testing:**
1. Open the developer center and navigate to edit an existing application
2. Verify the new checkbox appears in the app settings with appropriate label and description
3. Enable the checkbox and save the application settings
4. Reload the app edit page and verify the checkbox remains checked
5. Open a file with a specific name (e.g., "test-document.pdf") using the configured application
6. Verify the window title displays "test-document.pdf" instead of just the application name
7. Open multiple different files in the same application and verify each window shows its respective file name
8. Test the form change detection by toggling the checkbox and verifying the save/cancel buttons respond appropriately
9. Test the reset functionality by making changes and clicking cancel to verify the checkbox returns to its original state

**Automated Testing:**
Run the existing test suite to ensure no regressions were introduced in the app settings functionality.