# Agent App Interface

This app is primarily a standalone OpenAI-powered chat client.

## Storage Keys

- `openai-api-key` — user's OpenAI API token
- `openai-model` — selected OpenAI model name, default `gpt-5.4`
- `system-prompt` — current system prompt text
- `chat-messages` — JSON array of rendered chat messages
- `chat-stats` — JSON object like:
  ```json
  {
    "toolCalls": 3,
    "lastTool": "webfetch • https://example.com"
  }
  ```

## Tooling

Inside the app UI, the assistant currently exposes one local tool:

- `webfetch(url)` — fetches URL text content and returns up to the first ~50KB, using the SlopOS web fetch capability.

## Notes

- This app does not currently expose inter-app messaging.
- This app does not currently provide a cross-app API.
- Conversation state is stored locally in app storage.
