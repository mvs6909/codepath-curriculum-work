# Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/BasedHardware/omi/3094)
- **PR:** [BasedHardware/omi#3094](https://github.com/BasedHardware/omi/pull/3094)
- **Difficulty:** Medium

# Improve action items with categorization, timezone handling, and notification scheduling

## Motivation

The action items feature needs significant improvements to provide a better user experience and make tasks more actionable. Currently, all action items are displayed in a single list without proper categorization, making it difficult for users to focus on what's important. Additionally, due dates are not being converted to the user's local timezone, causing confusion when items are displayed. Finally, there's no notification system to remind users about upcoming action items with due dates.

These improvements will help users better manage their tasks by organizing them into logical categories (To Do, Done, Snoozed), displaying dates in their local timezone for clarity, and proactively reminding them about upcoming deadlines. This transforms action items from a passive list into an active task management system that helps users stay on top of their commitments.

## Current Behavior

The action items feature has several limitations:

1. All incomplete items are shown together regardless of age or relevance
2. DateTime values are stored and displayed in UTC without conversion to local timezone
3. No notification system exists to remind users about action items with due dates
4. The UI doesn't provide clear categorization to help users prioritize tasks
5. Date formatting doesn't adapt based on context (relative vs absolute dates)

**Reproduction Steps:**

1. Navigate to the Action Items page in the app
2. Observe that all incomplete items are displayed in a single "To-Do's" section
3. Create or view an action item with a due date
4. Notice the due date is displayed in UTC timezone rather than your local timezone
5. Wait until an action item's due date approaches
6. Observe: No notification is received to remind you about the upcoming task
7. Look at older action items (>3 days old) mixed with recent ones
8. Observe: No way to separate or "snooze" older items from current priorities

## Expected Behavior

The action items feature should provide an organized, timezone-aware experience with proactive notifications:

1. Items should be categorized into three tabs: "To Do" (recent/upcoming), "Done" (completed), and "Snoozed" (older than 3 days)
2. All DateTime values should be converted to the user's local timezone for display
3. Users should receive local notifications 1 hour before action items with due dates
4. The notification system should handle updates and deletions of action items appropriately
5. Date formatting should adapt based on context (show full dates in Snoozed tab, relative dates elsewhere)

![Expected](../assets/ISSUE-5-AFTER.mp4)

**Acceptance Criteria:**

- [ ] Action items are categorized into three tabs: To Do (items within 3 days), Done (completed items), and Snoozed (items older than 3 days)
- [ ] All DateTime fields (created_at, updated_at, due_at, completed_at) are converted from UTC to local timezone when displayed and back to UTC when stored
- [ ] Local notifications are scheduled 1 hour before the due time for action items with due dates
- [ ] Notifications are properly cancelled when action items are deleted or completed
- [ ] Notifications are rescheduled when action item due dates are updated
- [ ] The notification system works in background via Firebase Cloud Messaging data messages
- [ ] Date formatting shows full dates with times in the Snoozed tab, and relative dates (Today, Tomorrow, weekday, or month/day) in other contexts
- [ ] Analytics events track tab changes and action item completions with context

## Verification

**Manual Testing:**

1. Open the Action Items page and verify three tabs are visible: To Do, Done, Snoozed
2. Create action items with various due dates (within 3 days, completed, older than 3 days)
3. Verify items appear in the correct tabs based on their status and age
4. Check that all displayed dates match your local timezone
5. Create an action item with a due date 2 hours in the future
6. Verify a notification is scheduled (check device notification settings or wait for it to fire)
7. Update the action item's due date and verify the notification is rescheduled
8. Complete or delete the action item and verify the notification is cancelled
9. Compare date formatting between tabs - Snoozed should show full dates, others should show relative
10. Force close the app and verify notifications still fire at the scheduled time
11. Switch between tabs and verify the correct items are displayed in each

**Automated Testing:**

Run the existing test suite to ensure no regressions:
```bash
flutter test
```

**What to Look For:**

- Tab switching works smoothly with correct item counts
- Dates display in your local timezone (compare with system time)
- Notifications appear 1 hour before due times
- No duplicate notifications for the same action item
- Snoozed tab shows full date/time format (e.g., "Jan 15, 3:30 PM")
- Other tabs show relative format (e.g., "Today", "Tomorrow", "Mon")
- Analytics events fire when switching tabs and completing items

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx