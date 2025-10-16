# Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/gitlab-org/gitlab/206410)
- **PR:** [gitlab-org/gitlab#206410](https://gitlab.com/gitlab-org/gitlab/-/merge_requests/206410)
- **Issue:** N/A
- **Difficulty:** Easy-Medium

# Work item notes load in wrong order after page refresh when "Newest first" is selected

## Motivation

Users can sort work item notes (comments) by either "Oldest first" or "Newest first" to view discussions in their preferred order. This preference should persist across page refreshes to maintain a consistent user experience. Currently, there's a disconnect between the frontend sort preference and how notes are initially loaded from the backend, causing confusion when users refresh the page and see notes in an unexpected order.

This issue impacts user experience by breaking the expected behavior of persistent UI preferences. When users select a sort order, they expect that preference to be respected not just for the current session, but also when they return to the page. Fixing this ensures data consistency between frontend state and backend queries, which is a fundamental principle of web application development.

## Current Behavior

When a user changes the sort order of work item notes to "Newest first" while viewing a work item, the frontend correctly reverses the display order of already-loaded notes. However, when the page is refreshed, the backend loads notes in the default order (oldest first) regardless of the user's previously selected sort preference. This creates an inconsistent experience where the sort preference appears to be lost on page reload.

**Reproduction Steps:**
1. Create a work item (issue) with a large number of notes (e.g., 300+ comments) to make the sorting behavior clearly visible. You can run a script to generate a comments below.
2. Navigate to the work item page and observe that notes are displayed in the default order
3. Change the sort order to "Newest first" using the sort dropdown
4. Observe that the notes are now displayed with newest comments at the top
5. Refresh the browser page
6. Observe: The notes are now displayed in oldest-first order again, even though the sort dropdown still shows "Newest first" as selected, or the notes don't match the expected order based on the user's last selection

**Expected Result:** After refresh, notes should load from the backend in "Newest first" order, matching the user's last selected preference.

**Actual Result:** Notes load in the default "Oldest first" order, ignoring the user's previously selected sort preference.

## Expected Behavior

When a user selects a sort order for work item notes, that preference should be persisted and used when loading notes from the backend on subsequent page loads. The backend query should respect the user's sort order preference so that notes are fetched in the correct order from the database, rather than relying solely on frontend reordering of already-loaded data.

| User State | Alert Behavior | Video |
|------------|----------------|------------|
| **Before** |![Before](assets/gitlab-i3-before.gif) | 
| **After** | ![After](assets/gitlab-i3-after.gif) |

**Acceptance Criteria:**
- [ ] When "Newest first" is selected, refreshing the page loads notes from the backend in newest-first order
- [ ] When "Oldest first" is selected (or default), refreshing the page loads notes from the backend in oldest-first order
- [ ] The sort order preference persists across page refreshes and browser sessions
- [ ] The backend API query includes the appropriate sort order parameter based on user preference
- [ ] The displayed notes match the selected sort order immediately upon page load, without requiring frontend reordering

## Verification

**Manual Testing:**
1. Create a test work item with at least 50-100 notes (use the provided script or create manually)
2. Visit the work item page and verify notes are displayed in oldest-first order by default
3. Change the sort order to "Newest first" and verify the newest notes appear at the top
4. Refresh the page and verify that notes still appear in newest-first order immediately upon load
5. Change the sort order back to "Oldest first" and refresh again to verify oldest-first ordering persists
6. Open the work item in a new browser tab/window and verify the sort preference is maintained
7. Check browser network requests to confirm the backend API is being called with the correct sort order parameter

**Success Indicators:**
- Notes appear in the correct order immediately upon page load without visible reordering
- Network requests show the sort order parameter being sent to the backend
- User preference persists across page refreshes and new sessions

### Hints
1. You can use a script to generate comments by using the console `bundle exec rails console`
```ruby
issue = Issue.last
project = issue.project
user = User.first

(1..300).each do |i|
  Notes::CreateService.new(project, user, {
    noteable_type: 'Issue',
    noteable_id: issue.id,
    note: i.to_s
  }).execute
end
```
### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx
