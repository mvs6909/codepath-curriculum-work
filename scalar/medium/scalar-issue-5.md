# Overrides are incorrectly persisted to intermediate document state

## Motivation

The workspace store maintains multiple layers of document state to support different use cases: the original document (source of truth), the intermediate document (working copy), and overrides (temporary in-memory modifications). This separation is critical for maintaining data integrity and enabling features like reverting changes without losing temporary customizations.

Currently, when overrides are applied to a document, those changes are being written back to the intermediate state during save operations. This violates the design principle that overrides should remain purely in-memory and never modify the underlying document layers. This bug can lead to unintended persistence of temporary changes, making it impossible to distinguish between intentional edits and temporary overrides, and breaking the revert functionality.

## Current Behavior

When a document is added with overrides, or when overrides are modified, those changes are being propagated to the intermediate document state. This means that temporary override values become permanent changes in the working copy, defeating the purpose of having a separate override layer.

**Reproduction Steps:**

1. Create a workspace store and add a document with overrides that modify specific fields (e.g., `info.title` and `info.version`)
2. Make additional changes to the overridden fields through the document proxy
3. Call `saveDocument()` to persist changes to the intermediate state
4. Export the document and observe that override values are included in the exported content
5. Call `revertDocumentChanges()` to revert to the intermediate state
6. Observe: The overridden values are reverted instead of being preserved, and the intermediate state contains override values instead of only intentional edits

Expected: Overrides should remain in-memory only, the intermediate state should only contain intentional edits, and reverting should restore the intermediate state while preserving overrides.

Actual: Overrides are written to the intermediate state, making them indistinguishable from regular edits, and revert operations incorrectly affect override values.

## Expected Behavior

Overrides should be maintained as a completely separate, in-memory layer that never affects the intermediate or original document states. When documents are saved, exported, or reverted, only the intermediate state should be modified—overrides should remain isolated and continue to be applied on top of the document state for display purposes only.

**Acceptance Criteria:**

- [ ] Saving a document does not write override values to the intermediate state
- [ ] Exporting a document returns only the intermediate state without override values
- [ ] Reverting document changes restores the intermediate state while preserving all override values
- [ ] The exported workspace includes an `overrides` field that maintains the override state separately
- [ ] All existing tests pass and new tests verify that overrides remain isolated from intermediate state

## Verification

Run the test suite to verify the fix:

```bash
pnpm test packages/workspace-store/src/client.test.ts
```

Key test cases to verify:

1. **Override isolation during save**: Add a document with overrides, modify fields, save the document, then export it. The exported JSON should not contain override values.

2. **Override preservation during revert**: Add a document with overrides, make changes to overridden fields, then revert. The override values should remain unchanged while other edits are reverted.

3. **Workspace export includes overrides**: Export the entire workspace and verify that the `overrides` field is present in the exported data structure, maintaining override state separately from document state.

4. **Override application on document access**: Access a document through the workspace and verify that override values are visible in the proxied document, but the underlying intermediate state remains unmodified.

All tests should pass, demonstrating that overrides are properly isolated from the intermediate document state while still being applied for runtime access.