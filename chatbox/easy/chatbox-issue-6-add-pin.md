# Metadata

This issue DNE in the chatbox repo as of now. I have created it.
PR - https://github.com/mvs6909/chatbox/pull/6

# Add Pin Chat Functionality to Keep Important Conversations Accessible

## Motivation

Users often have multiple chat sessions active, and some conversations are more important or frequently accessed than others. As the number of chat sessions grows, it becomes difficult to quickly find and access these important conversations. Users must scroll through their entire chat list to locate frequently-used sessions, which wastes time and disrupts their workflow.

Providing a pin functionality allows users to mark up to 3 conversations as favorites, keeping them at the top of the session list for quick access. This is a common pattern in messaging applications (Slack, Discord, WhatsApp) and significantly improves the user experience for power users who maintain many concurrent conversations.

The limit of 3 pinned chats prevents the pinned section from becoming cluttered while still providing enough slots for users to keep their most important conversations accessible.

## Current Behavior

All chat sessions appear in the session list in their default order (likely creation order or last activity). There is no way to prioritize or mark certain conversations as more important. Users must manually search through the list to find frequently-accessed conversations.

**Reproduction Steps:**
1. Launch the ChatBox application
2. Create several chat sess# Add Pin Chat Functionality to Keep Important Conversations Accessible

## Motivation

Users often have multiple chat sessions active, and some conversations are more important or frequently accessed than others. As the number of chat sessions grows, it becomes difficult to quickly find and access these important conversations. Users must scroll through their entire chat list to locate frequently-used sessions, which wastes time and disrupts their workflow.

Providing a pin functionality allows users to mark up to 3 conversations as favorites, keeping them at the top of the session list for quick access. This is a common pattern in messaging applications (Slack, Discord, WhatsApp) and significantly improves the user experience for power users who maintain many concurrent conversations.

The limit of 3 pinned chats prevents the pinned section from becoming cluttered while still providing enough slots for users to keep their most important conversations accessible.

## Current Behavior

All chat sessions appear in the session list in their default order (likely creation order or last activity). There is no way to prioritize or mark certain conversations as more important. Users must manually search through the list to find frequently-accessed conversations.

**Reproduction Steps:**
1. Launch the ChatBox application
2. Create several chat sessions (e.g., 5-10 sessions)
3. Try to keep track of your most important conversations
4. Observe: All sessions appear in the same order with no way to prioritize them
5. Important conversations get lost among less frequently used ones

## Expected Behavior

Users should be able to pin up to 3 chat sessions, which will appear at the top of the session list with a visual indicator. Pinned sessions should remain at the top regardless of when they were last used. Users should receive feedback when attempting to pin more than 3 sessions.

**Acceptance Criteria:**
- [ ] Each session item displays a pin button (pushpin icon)
- [ ] Clicking the pin button pins the session (max 3 pins allowed)
- [ ] Pinned sessions appear at the top of the session list
- [ ] Pinned sessions display a filled pin icon, unpinned sessions show an outlined pin icon
- [ ] Clicking the pin button on a pinned session unpins it
- [ ] Attempting to pin a 4th session shows a toast notification: "Maximum 3 chats can be pinned"
- [ ] Pin state persists across application restarts

## Verification

**Manual Testing:**

1. **Test Pin Functionality:**
   - Start the ChatBox application
   - Create 4-5 test chat sessions
   - Locate the pin button (outlined pin icon) next to each session
   - Click the pin button and verify:
     - The icon changes to a filled pin (solid, primary colored)
     - The session moves to the top of the list

2. **Test Multiple Pins:**
   - Pin a second session
   - Verify both pinned sessions appear at the top
   - Pin a third session
   - Verify all three pinned sessions appear at the top in correct order

3. **Test Pin Limit:**
   - With 3 sessions already pinned, attempt to pin a 4th session
   - Verify a toast notification appears: "Maximum 3 chats can be pinned"
   - Verify the 4th session is NOT pinned

4. **Test Unpin:**
   - Click the pin button on a pinned session
   - Verify the session is unpinned (icon changes to outlined)
   - Verify the session moves down in the list (below other pinned sessions)
   - Verify you can now pin a different session

5. **Test Persistence:**
   - Pin 2-3 sessions
   - Close and restart the application
   - Verify the pinned sessions remain pinned and at the top of the list

6. **Test Sorting:**
   - Verify pinned sessions always appear before unpinned sessions
   - Create new sessions and verify they appear below pinned sessions

**Expected Results:**
- Pin/unpin functionality works smoothly
- Visual feedback is clear (filled vs outlined pin icon)
- Maximum of 3 sessions can be pinned at once
- Toast notification appears when trying to exceed limit
- Pinned sessions persist across app restarts
- List sorting works correctly

## Expected Behavior

Users should be able to pin up to 3 chat sessions, which will appear at the top of the session list with a visual indicator. Pinned sessions should remain at the top regardless of when they were last used. Users should receive feedback when attempting to pin more than 3 sessions.

**Acceptance Criteria:**
- [ ] Each session item displays a pin button (pushpin icon)
- [ ] Clicking the pin button pins the session (max 3 pins allowed)
- [ ] Pinned sessions appear at the top of the session list
- [ ] Pinned sessions display a filled pin icon, unpinned sessions show an outlined pin icon
- [ ] Clicking the pin button on a pinned session unpins it
- [ ] Attempting to pin a 4th session shows a toast notification: "Maximum 3 chats can be pinned"
- [ ] Pin state persists across application restarts

## Verification

**Manual Testing:**

1. **Test Pin Functionality:**
   - Start the ChatBox application
   - Create 4-5 test chat sessions
   - Locate the pin button (outlined pin icon) next to each session
   - Click the pin button and verify:
     - The icon changes to a filled pin (solid, primary colored)
     - The session moves to the top of the list

2. **Test Multiple Pins:**
   - Pin a second session
   - Verify both pinned sessions appear at the top
   - Pin a third session
   - Verify all three pinned sessions appear at the top in correct order

3. **Test Pin Limit:**
   - With 3 sessions already pinned, attempt to pin a 4th session
   - Verify a toast notification appears: "Maximum 3 chats can be pinned"
   - Verify the 4th session is NOT pinned

4. **Test Unpin:**
   - Click the pin button on a pinned session
   - Verify the session is unpinned (icon changes to outlined)
   - Verify the session moves down in the list (below other pinned sessions)
   - Verify you can now pin a different session

5. **Test Persistence:**
   - Pin 2-3 sessions
   - Close and restart the application
   - Verify the pinned sessions remain pinned and at the top of the list

6. **Test Sorting:**
   - Verify pinned sessions always appear before unpinned sessions
   - Create new sessions and verify they appear below pinned sessions

**Expected Results:**
- Pin/unpin functionality works smoothly
- Visual feedback is clear (filled vs outlined pin icon)
- Maximum of 3 sessions can be pinned at once
- Toast notification appears when trying to exceed limit
- Pinned sessions persist across app restarts
- List sorting works correctly

### Submission

Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx