## Metadata (not to include in the student issue)

- PR Link: https://github.com/scalar/scalar/pull/6503/files
- Issue link: NA
- Tool: https://openbootstrap.onrender.com/pr/scalar/scalar/6503
- Diufficulty: Easy

# Search fails to match queries containing single-character words

## Motivation

Search functionality is a critical feature for API documentation, allowing developers to quickly find the endpoints and information they need. When search fails to match obvious queries, it creates a frustrating user experience and makes the documentation feel broken or incomplete.

Currently, the search implementation has a configuration that prevents matching when individual words in the query are too short. While this was intended to reduce noise from single-character matches, it has the unintended consequence of rejecting entire queries that contain common single-character words like "a", "I", or "v". For example, searching for "Get a token" fails to match an endpoint titled "Get a token", forcing users to remove the article and search for "Get token" instead. This behavior is unintuitive and degrades the search experience.

## Current Behavior

The search functionality fails to return results when the search query contains words with 1 or 2 characters, even when those queries should match existing content perfectly.

**Reproduction Steps:**
1. Navigate to the API reference search interface
2. Locate an endpoint with a title containing a single-character word (e.g., "Get a token")
3. Enter the exact title "Get a token" into the search box
4. Observe: No results are returned, even though an exact match exists
5. Remove the single-character word and search for "Get token"
6. Observe: The endpoint now appears in the results

**Expected vs Actual:**
- Expected: Searching "Get a token" should match an endpoint titled "Get a token"
- Actual: The search returns no results

## Expected Behavior

The search should successfully match queries containing single-character words, treating them as valid parts of the search phrase rather than filtering them out. Users should be able to search using natural language, including articles and other short words, and receive relevant results.

**Acceptance Criteria:**
- [ ] Searching for "Get a token" successfully matches an endpoint titled "Get a token"
- [ ] Searching for phrases with single-character words returns appropriate results
- [ ] Search quality is maintained - results are still relevant and not flooded with noise
- [ ] Comprehensive tests verify search behavior with various query patterns including single-character words
- [ ] Existing search functionality continues to work as expected for multi-character queries
- [ ] Unit tests are added to validate the new functionality

## Verification

**Manual Testing:**
1. Create or locate an API endpoint with a title containing a single-character word
2. Search for the complete title including the single-character word
3. Verify the endpoint appears in the search results
4. Test with various queries containing 1-2 character words to ensure consistent behavior

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx