# Metadata

This issue DNE in the chatbox repo as of now. I have created it.
PR - https://github.com/mvs6909/chatbox/pull/7

# Add Tooltips to Icon Buttons for Better User Experience

## Motivation

The ChatBox application uses icon-only buttons throughout its interface (such as ⋮, ⚙️, 🧹, etc.). While icons provide a clean, minimal design, they can be confusing for new users who may not immediately understand what each button does. Without text labels or tooltips, users must rely on trial and error to discover functionality, which creates a poor first-time user experience.

Adding tooltips to icon buttons is a standard UX practice that improves discoverability and accessibility. Tooltips provide contextual help exactly when users need it - at the moment they hover over a button - without cluttering the interface. This is especially important for an AI chat application where users need to quickly understand available actions like regenerating responses, clearing messages, or accessing settings.

## Current Behavior

Icon buttons throughout the application have no tooltips or hover hints. Users must click buttons to discover their functionality, which can be frustrating and may lead to accidental actions.

**Reproduction Steps:**
1. Launch the ChatBox application
2. Hover over any icon button (settings gear icon, three-dot menu, cleaning icon, etc.)
3. Observe: No tooltip appears to explain what the button does
4. User must click the button to discover its purpose

**Affected Components:**
- Main application toolbar icons
- Session list item action buttons
- Message block action buttons (refresh, more options, save)
- Sidebar menu icons (new chat, settings, version info)

## Expected Behavior

When a user hovers over any icon button in the application, a descriptive tooltip should appear near the cursor explaining what the button does. The tooltip should use clear, concise language and appear/disappear smoothly as the user moves their mouse.

**Acceptance Criteria:**
- [ ] All icon buttons in App.tsx display tooltips on hover
- [ ] All icon buttons in Block.tsx (message actions) display tooltips on hover
- [ ] All icon buttons in SessionItem.tsx display tooltips on hover
- [ ] Tooltip text is descriptive and uses action-oriented language (e.g., "Open settings" not just "Settings")
- [ ] Tooltips appear near the hovered element and don't obstruct other UI elements
- [ ] Tooltips use Material-UI's Tooltip component for consistency

## Verification

**Manual Testing:**
1. Start the ChatBox application in development mode
2. Navigate to the main chat interface
3. Hover over the ChatBox logo icon and verify tooltip shows "ChatBox"
4. Hover over the settings gear icon and verify tooltip shows "Open settings"
5. Hover over the "+" icon and verify tooltip shows "Create new chat"
6. Hover over the info icon and verify tooltip shows "Check for updates"
7. In the message area, hover over the cleaning icon and verify tooltip shows "Clear all messages"
8. In the message area, hover over the chat bubble icon and verify tooltip shows "Current conversation"
9. Create a test message and hover over it to reveal action buttons
10. Hover over the refresh icon and verify tooltip shows "Regenerate response"
11. Hover over the three-dot menu icon and verify tooltip shows "More actions"
12. While editing a message, hover over the checkmark and verify tooltip shows "Save changes"
13. In the session list, hover over the chat bubble icon and verify tooltip shows "Chat session"
14. In the session list, hover over the horizontal dots icon and verify tooltip shows "Session options"

**Expected Results:**
- All icon buttons display appropriate tooltips on hover
- Tooltips appear smoothly without lag
- Tooltip text is clear and describes the button's action
- Tooltips disappear when the mouse moves away
- No console errors or warnings

### Summary of Tooltips Added:
- **6 tooltips** in App.tsx (sidebar and message area)
- **3 tooltips** in Block.tsx (message action buttons)
- **2 tooltips** in SessionItem.tsx (session list items)
- **Total: 11 tooltips** across 3 components

All tooltips use Material-UI's `<Tooltip>` component for consistent styling and behavior with the existing design system.

### Submission

Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx