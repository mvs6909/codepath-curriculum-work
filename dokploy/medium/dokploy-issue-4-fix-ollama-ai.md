## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/Dokploy/dokploy/2410)
- **PR:** [dokploy/dokploy#2410](https://github.com/Dokploy/dokploy/pull/2410)
- **Issue:** N/A

# Fix Ollama AI provider integration

## Motivation

The AI feature in Dokploy currently has hardcoded assumptions that work well for OpenAI but break when users try to configure other AI providers like Ollama. Ollama is a popular local AI model runner that **doesn't require API keys** and uses different API endpoints than cloud-based providers. This creates a poor user experience where users cannot successfully configure Ollama, and the interface shows irrelevant fields and validation errors.

Supporting multiple AI providers with different authentication schemes is essential for giving users flexibility in their AI infrastructure choices. Some users prefer local models for privacy or cost reasons, while others use cloud providers. The system should gracefully handle these differences rather than forcing all providers into an OpenAI-shaped box.

Read more: [Ollama](https://ollama.com/)

## Current Behavior

The AI configuration form has several issues that prevent proper Ollama integration:

1. The API key field is always required, even though Ollama doesn't use authentication
2. The model field is stuck in a loading loop.
3. The models list endpoint assumes OpenAI's `/models` endpoint structure
4. The system uses an unofficial Ollama provider library instead of the recommended one
5. When the models list is empty, there's no user feedback

**Reproduction Steps:**

1. Download Ollama locally and install any model
2. Navigate to the AI settings page in Dokploy
3. Try to configure a new AI provider with Ollama settings:
   - Name: "Local Ollama"
   - API URL: "http://localhost:11434"
   - Leave API Key empty (as Ollama doesn't require it)
   - Try to select a model
4. Observe: Form validation fails requiring an API key
5. If you enter a dummy API key, observe: The models endpoint fails because it's calling the wrong endpoint (`/models` instead of `/api/tags`)
6. Observe: The model field fails to load
7. Observe: No feedback message when the models list is empty

## Expected Behavior

The AI configuration should intelligently handle different provider types:

- Ollama providers (detected by `:11434` port or "ollama" in URL) should not require an API key
- The model field should start empty rather than pre-selecting an OpenAI-specific model
- The system should call the correct API endpoint for each provider type (Ollama uses `/api/tags`, others use `/models`)
- Users should see a helpful message when no models are available
- The system should use the officially recommended Ollama provider library

**Acceptance Criteria:**

- [ ] API key field is conditionally required: mandatory for most providers, optional for Ollama
- [ ] Model field starts empty with no pre-selected value
- [ ] Provider detection correctly identifies Ollama by URL pattern (`:11434` or contains "ollama")
- [ ] API Field is not shown for a Ollama model
- [ ] Models endpoint calls the correct API path based on provider type
- [ ] Empty models list shows a user-friendly message
- [ ] System uses the `ai-sdk-ollama` package instead of `ollama-ai-provider` (Need to install new package)
- [ ] All AI provider SDK dependencies are updated to their latest compatible versions

## Verification

![Expected Results](../assets/issue4-image-1-ai%20setup.png)

**Manual Testing:**

1. Configure an Ollama provider:
   - Set API URL to `http://localhost:11434` (requires Ollama running locally)
   - Verify the API key field is not shown or not required
   - Verify models load from the `/api/tags` endpoint
   - Verify you can select and save an Ollama model
   - Test the AI feature with the configured Ollama provider

2. Configure an OpenAI provider:
   - Set API URL to `https://api.openai.com/v1`
   - Verify the API key field is required
   - Verify models load from the `/models` endpoint
   - Verify the form validates correctly

3. Test edge cases:
   - Try submitting the form with an empty models list
   - Verify the "No models available" message appears
   - Test with various Ollama URL formats (localhost:11434, custom-host:11434, urls containing "ollama")

**Success Indicators:**
- Ollama can be configured without an API key
- Models load correctly for both Ollama and OpenAI providers
- Form validation adapts based on the provider type
- No TypeScript errors or test failures

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx