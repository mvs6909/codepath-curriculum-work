## Metadata (not to include in the student issue)

PR Link - https://github.com/scalar/scalar/pull/6661/files
Issue link - https://github.com/scalar/scalar/issues/6594
Tool - https://openbootstrap.onrender.com/pr/scalar/scalar/6661

# Title: Unable to encode non-Latin1 characters for BasicAuthehtication for API Client

## Motivation

HTTP Basic Authentication requires encoding credentials in Base64 format. Currently, the application uses JavaScript's native `btoa()` function for this encoding. However, `btoa()` has a significant limitation: it only supports Latin1 (ISO-8859-1) characters. When users attempt to authenticate with credentials containing Unicode characters (such as Polish characters like `żółć`, Cyrillic characters, or emoji), the application throws an `InvalidCharacterError` and fails to send the request.

This limitation affects international users who may have passwords or usernames containing non-ASCII characters. Supporting Unicode characters in authentication credentials is essential for providing a truly global, accessible API client that works for users regardless of their language or character set preferences.

## Current Behavior

The application fails when attempting to encode Basic Authentication credentials that contain characters outside the Latin1 range. The native `btoa()` function throws an error, preventing the request from being sent.

**Reproduction Steps:**

1. Run `pnpm dev:void-server` to run a void server adn copy the `url` for the server. This will be used to send the request for reproducing this issue.
1. On a new terminal, run `pnpm dev:client` and open the client on your `localhost`.
2. Configure a request to
  1. `URL` - void-server URL
  2. `Auth Type`: `httpBasic`
  3. `username`: `admin`
  4. `password`: `żółć` (non-latin1 characters.)
2. Set the username to `admin` and password to `żółć` (or any string containing non-Latin1 characters like Cyrillic `тест` or other Unicode characters)
3. Attempt to send the request
4. Observe: The application throws an error in the browser console: `InvalidCharacterError: Failed to execute 'btoa' on 'Window': The string to be encoded contains characters outside of the Latin1 range.`
5. Observe: The request is not sent and authentication fails
6. Try the request again with 
  1. `URL` - void-server URL
  2. `Auth Type`: `httpBasic`
  3. `username`: `admin`
  4. `password`: `password`
7. Observe the request send successfully.

## Expected Behavior

The application should successfully encode credentials containing Unicode characters and send requests with proper Basic Authentication headers, regardless of the character set used in the username or password.

**Acceptance Criteria:**

- [ ] Basic Authentication works with Unicode characters in both username and password fields
- [ ] No `InvalidCharacterError` is thrown when using non-Latin1 characters
- [ ] The Authorization header is correctly formatted with properly encoded Base64 credentials
- [ ] All existing authentication functionality continues to work as before. All `btoa` references are migrated to Base64 where relevant.
- [ ] Unit tests verify that Unicode characters (e.g., `żółć`, `тест`) are correctly encoded. 

## Verification

**Manual Testing:**
1. Configure an API endpoint with Basic Authentication
2. Set credentials using Unicode characters (e.g., username: `żółć`, password: `тест`)
3. Send the request
4. Verify the request completes without errors
5. Inspect the Authorization header to confirm it contains valid Base64-encoded credentials

**Automated Testing:**
1. Run the test suite: `pnpm test`
2. Verify that all existing authentication tests pass
3. Verify that new tests for Unicode character encoding pass
4. Check that the encoded credentials match the expected Base64 output for Unicode strings

## Relevant resources

1. Common issue with `btoa()` - https://stackoverflow.com/questions/23223718/failed-to-execute-btoa-on-window-the-string-to-be-encoded-contains-characte
2. Library for base64 that was used in the original fix - https://github.com/dankogai/js-base64