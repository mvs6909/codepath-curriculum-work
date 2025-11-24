## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/heyputer/puter/1156)
- **PR:** [puter/puter#1156](https://github.com/heyputer/puter/pull/1156)
- **Issue:** N/A

# Enhance ai command to execute shell commands from AI responses

## Motivation

The current `ai` command in the Puter shell can only respond with text-based answers to user queries. However, when users ask the AI to perform actions like "create a directory named test" or "list all files", the AI can only describe what command to run rather than actually executing it. This creates friction in the user experience, requiring users to manually copy and execute the suggested commands.

By enabling the `ai` command to recognize and execute shell commands directly, we can create a more seamless and powerful interface where users can interact with their system using natural language. This bridges the gap between conversational AI and system automation, making the shell more accessible to users who may not remember exact command syntax.

## Current Behavior

The `ai` command currently only outputs text responses from the AI model. When a user asks the AI to perform an action that requires a shell command, the AI can only respond with a textual description or suggestion, but cannot actually execute the command.

**Reproduction Steps:**
1. Open the Puter shell terminal. (If you don't see the terminal app, go to the App center and search)
2. Run the command: `ai "create a directory named test"`
3. Observe: The AI responds with text explaining how to create a directory or suggesting the command, but does not actually create the directory
4. Run `ls` to verify that no directory was created
5. Expected: The directory should have been created automatically
6. Actual: Only a text response is provided, no directory exists

## Expected Behavior

The `ai` command should be able to detect when the AI response contains a shell command that should be executed, extract that command, prompt the user for confirmation, and execute it if confirmed. The AI should be instructed via system prompts to wrap executable commands in a special marker format that can be parsed and extracted.

**Acceptance Criteria:**
- [ ] The AI receives system instructions to wrap executable commands in a special marker format (e.g., `%%%command%%%`)
- [ ] The `ai` command parses AI responses to detect and extract wrapped commands using pattern matching
- [ ] When a command is detected, the user is prompted for confirmation before execution (y/n)
- [ ] If the user confirms, the command is executed using the shell's pipeline execution mechanism
- [ ] If the user declines or provides any response other than 'y', the command execution is cancelled with an appropriate message
- [ ] Regular text responses (without commands) continue to work as before, displaying the AI's message
- [ ] The command input stream is configured to read user confirmation synchronously

## Verification

**Manual Testing:**
1. Run `ai create a directory named mytest` in the Puter shell
2. Verify that the AI responds with a message and a command wrapped in markers
3. Verify that you are prompted: `Execute command: 'mkdir mytest' (y/n):`
4. Type `y` and press Enter
5. Verify that the command executes and the directory is created
6. Run `ls` to confirm the `mytest` directory exists
7. Run `ai what is the weather like` (a non-command query)
8. Verify that the AI responds with text only, no command execution prompt
9. Run `ai list all files` and type `n` when prompted
10. Verify that the command is cancelled and not executed

**Testing Edge Cases:**
1. Test with various command types (mkdir, ls, cat, etc.)
2. Test declining command execution to ensure cancellation works
3. Test queries that don't require commands to ensure normal behavior is preserved
4. Verify error handling when a command fails to execute

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx