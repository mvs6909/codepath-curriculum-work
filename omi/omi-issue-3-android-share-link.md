# Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/BasedHardware/omi/2928)
- **PR:** [BasedHardware/omi#2928](https://github.com/BasedHardware/omi/pull/2928)
- **Difficulty:** Easy

# Add share option for Android in settings drawer

## Motivation

The Omi app currently only provides a share option for the iPhone version in the settings drawer, which creates an inconsistent user experience for Android users. When Android users open the settings, they see a "Share Omi for iPhone" option, which is confusing and not relevant to their platform. 

This change will improve the user experience by showing platform-appropriate share options to users based on their device. Android users should be able to easily share the Android version of the app with others, just as iOS users can share the iOS version. This helps with organic growth and user acquisition by making it simple for users to recommend the app to friends on the same platform.

## Current Behavior

The settings drawer displays a "Share Omi for iPhone" option that links to the App Store, regardless of which platform the user is on. This means Android users see an option to share the iPhone version of the app, which is not useful for their contacts who may also be on Android.

**Reproduction Steps:**
1. Open the Omi app on an Android device
2. Navigate to the settings drawer
3. Scroll to the "Share & Get" section
4. Observe: Only "Share Omi for iPhone" option is visible, which links to the Apple App Store
5. Expected: Android users should see a "Share Omi for Android" option that links to the Google Play Store

## Expected Behavior

The settings drawer should display platform-specific share options based on the user's device. iOS users should see "Share Omi for iPhone" linking to the App Store, while Android users should see "Share Omi for Android" linking to the Google Play Store. Each option should be visible only on its respective platform.

**Acceptance Criteria:**
- [ ] iOS users see only the "Share Omi for iPhone" option in the settings drawer
- [ ] Android users see only the "Share Omi for Android" option in the settings drawer
- [ ] The Android share option uses an appropriate icon (Google Play icon)
- [ ] Tapping the Android share option opens the share dialog with the correct Google Play Store link
- [ ] The share functionality works correctly on both platforms without errors

## Verification

**Manual Testing:**
1. Run the app on an Android device or emulator
2. Open the settings drawer and navigate to the "Share & Get" section
3. Verify that "Share Omi for Android" is displayed (not "Share Omi for iPhone")
4. Tap the share option and confirm the share dialog opens with the Google Play Store link
5. Run the app on an iOS device or simulator
6. Open the settings drawer and verify that "Share Omi for iPhone" is displayed (not the Android option)
7. Confirm both platforms show only their respective share options

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx