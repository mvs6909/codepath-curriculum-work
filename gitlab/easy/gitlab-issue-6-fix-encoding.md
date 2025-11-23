# Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/gitlab-org/gitlab/207561)
- **PR:** [gitlab-org/gitlab#207561](https://gitlab.com/gitlab-org/gitlab/-/merge_requests/207561)
- **Issue:** https://gitlab.com/gitlab-org/gitlab/-/issues/566950
- **Difficulty:** Easy

# Fix encoding error when processing non-UTF-8 User-Agent headers

## Motivation

Web applications must handle HTTP requests from a wide variety of clients, including those that may send malformed or non-UTF-8 encoded headers. The User-Agent header, in particular, can contain arbitrary byte sequences that don't conform to UTF-8 encoding standards. When our application attempts to process these non-UTF-8 User-Agent strings with UTF-8 regular expressions (used by the device detection library), it causes an `Encoding::CompatibilityError` that results in a 500 Internal Server Error.

This is a critical bug because it allows any client to crash user sessions by simply sending a malformed User-Agent header. This affects the reliability and availability of the application, as legitimate users could be denied service, and it creates a potential vector for denial-of-service attacks. Proper encoding handling is essential for building robust web applications that can gracefully handle unexpected input.

## Current Behavior

When a request is made with a User-Agent header containing non-UTF-8 byte sequences, the application crashes with an `Encoding::CompatibilityError` during the active session creation process. The error occurs when the device detector library attempts to match UTF-8 regular expressions against an ASCII-8BIT encoded string.

**Reproduction Steps:**

1. Set up a local GitLab development environment (GDK)
2. Log in to the application and obtain a valid `_gitlab_session` cookie value from your browser's developer tools under Application > Cookies
3. Execute the following curl command (replace the session ID with your actual session):
   ```bash
   curl 'http://localhost:3000/gitlab-duo/test/-/autocomplete_sources/members?type=WorkItem&type_id=566950&search=Leeticket' \
     -H $'User-Agent: \xe2\x80\xae[tcejbO tcejbo]' \
     -H "Cookie: _gitlab_session=YOUR_SESSION_ID" -v
   ```
4. Observe: The request returns HTTP 500 Internal Server Error
5. Check the error logs and observe: `Encoding::CompatibilityError: incompatible encoding regexp match (UTF-8 regexp with ASCII-8BIT string)` originating from `app/models/active_session.rb:83` when attempting to access `client.name`

## Expected Behavior

The application should gracefully handle User-Agent headers with any encoding, converting them to UTF-8 before processing. Requests with non-UTF-8 User-Agent headers should complete successfully without raising encoding errors, and the session should be created normally with the browser/device information properly extracted.

**Acceptance Criteria:**

- [ ] Requests with non-UTF-8 encoded User-Agent headers return HTTP 200 (or appropriate non-500 status) instead of crashing
- [ ] Active sessions are created successfully even when User-Agent contains non-UTF-8 byte sequences
- [ ] The device detection logic (client.name, client.os_name, etc.) operates on properly encoded UTF-8 strings
- [ ] Existing functionality for standard UTF-8 User-Agent headers remains unchanged
- [ ] Tests are added to verify handling of non-UTF-8 User-Agent strings

## Verification

**Manual Testing:**

1. Apply your fix to the codebase
2. Start your local GitLab development environment
3. Obtain a valid session cookie from your browser
4. Execute the same curl command from the reproduction steps:
   ```bash
   curl 'http://gdk.test:3000/gitlab-duo/test/-/autocomplete_sources/members?type=WorkItem&type_id=566950&search=Leeticket' \
     -H $'User-Agent: \xe2\x80\xae[tcejbO tcejbo]' \
     -H "Cookie: _gitlab_session=YOUR_SESSION_ID" -v
   ```
5. Verify the request returns HTTP 200 with valid JSON response
6. Test with various other non-UTF-8 byte sequences in the User-Agent header to ensure robustness

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx
