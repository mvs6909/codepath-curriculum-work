## Metadata (not to include in the student issue)

- PR Link: https://github.com/scalar/scalar/pull/6735/files
- Issue link: NA
- Tool: https://openbootstrap.onrender.com/pr/scalar/scalar/6735
- Diufficulty: Easy

NEED REPRO VERIFICATION

# Documents with `required: null` fail OpenAPI schema validation

## Motivation

OpenAPI documents sometimes contain schemas where the `required` field is set to `null` instead of an empty array or being omitted entirely. According to the OpenAPI specification, the `required` field should be an array of strings listing required properties. When `required` is `null`, the schema validation fails, causing the entire document rendering to break.

This creates a poor user experience where otherwise valid API documentation becomes completely unusable due to a minor data quality issue. By normalizing `required: null` to `required: []` during document processing, we can make the system more resilient to these edge cases and ensure documents render successfully even when they contain this malformed data.

## Current Behavior

When an OpenAPI document contains a schema with `required: null`, the validation process fails and prevents the entire document from rendering. The system does not gracefully handle this edge case, treating `null` as an invalid value rather than normalizing it to an empty array.

**Reproduction Steps:**
1. Create an OpenAPI document with a schema that has `required: null` (e.g., an object schema with properties but `required` set to `null`)
2. Attempt to process or validate this document through the workspace-store
3. Observe: The validation fails and the document cannot be rendered
4. Expected: The document should be processed successfully with `required` treated as an empty array

## Expected Behavior

The system should automatically normalize `required: null` to `required: []` during document processing. This normalization should happen in the `cleanUp` plugin, which is designed to fix common data quality issues in OpenAPI documents. After this change, documents with `required: null` should pass validation and render successfully.

**Acceptance Criteria:**
- [ ] The `cleanUp` plugin detects when a schema has `required: null`
- [ ] The plugin converts `required: null` to `required: []` for object schemas with properties
- [ ] Documents that previously failed validation due to `required: null` now process successfully
- [ ] The normalization only applies to schemas that have a `properties` field (object schemas)
- [ ] Tests verify the conversion works correctly

## Verification

**Manual Testing:**
1. Create a test OpenAPI document with a schema containing `required: null`
2. Process the document through the workspace-store with the `cleanUp` plugin enabled
3. Verify the document validates successfully and the `required` field is now an empty array

**Automated Testing:**
Run the test suite to verify the new test case passes:
```bash
pnpm test plugins.test.ts
```

Look for the test that verifies `required: null` is converted to `required: []` for object schemas. The test should pass, confirming that the normalization logic works correctly and the schema structure is preserved while fixing the invalid `null` value.

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx