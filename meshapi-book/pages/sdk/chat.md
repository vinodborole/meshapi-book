---
type: Web Page
title: Chat Completions - Mesh API
description: Send chat completion requests, build multi-turn conversations, and use
  prompt templates.
resource: https://developers.meshapi.ai/sdk/chat
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Basic request

- Python
- Node.js
- Go

```
from meshapi import MeshAPI, ChatCompletionParams, ChatMessage
client = MeshAPI(base_url="https://api.meshapi.ai", token="rsk_...")
resp = client.chat.completions.create(
    ChatCompletionParams(
        model="openai/gpt-4o-mini",
        messages=[ChatMessage(role="user", content="What is the capital of France? Reply in one word.")],
        max_tokens=10,
        temperature=0,
    )
)
print(resp.choices[0].message.content)
```
```
import { MeshAPI } from "meshapi-node-sdk";
const client = new MeshAPI({ baseUrl: "https://api.meshapi.ai", token: "rsk_..." });
const resp = await client.chat.completions.create({
  model: "openai/gpt-4o-mini",
  messages: [{ role: "user", content: "What is 2 + 2? Reply in one word." }],
  max_tokens: 10,
  temperature: 0,
});
console.log(resp.choices[0].message?.content);
```
```
import (
    "context"
    meshapi "github.com/aifiesta/meshapi-go-sdk"
)
model := "openai/gpt-4o-mini"
maxTokens := 10
resp, err := client.Chat.Completions.Create(ctx, meshapi.ChatCompletionParams{
    Model:     &model,
    Messages:  []meshapi.ChatMessage{{Role: "user", Content: "Reply with the single word: pong"}},
    MaxTokens: &maxTokens,
})
if err != nil {
    log.Fatal(err)
}
fmt.Println(*resp.Choices[0].Message.Content)
```
## Multi-turn conversation

Pass prior messages in the`messages` array to give the model conversation context.
- Python
- Node.js
- Go

```
resp = client.chat.completions.create(
    ChatCompletionParams(
        model="openai/gpt-4o-mini",
        messages=[
            ChatMessage(role="user", content="My favourite color is blue. Remember this."),
            ChatMessage(role="assistant", content="Got it! Your favourite color is blue."),
            ChatMessage(role="user", content="What is my favourite color? Reply in 3 words max."),
        ],
        max_tokens=20,
        temperature=0,
    )
)
print(resp.choices[0].message.content)
```
```
const resp = await client.chat.completions.create({
  model: "openai/gpt-4o-mini",
  messages: [
    { role: "user", content: "My favourite color is blue. Remember this." },
    { role: "assistant", content: "Got it! Your favourite color is blue." },
    { role: "user", content: "What is my favourite color? Reply in 3 words max." },
  ],
  max_tokens: 20,
  temperature: 0,
});
```
```
model := "openai/gpt-4o-mini"
maxTokens := 20
resp, err := client.Chat.Completions.Create(ctx, meshapi.ChatCompletionParams{
    Model: &model,
    Messages: []meshapi.ChatMessage{
        {Role: "system", Content: "You are a concise assistant. One sentence only."},
        {Role: "user", Content: "What is the capital of France?"},
    },
    MaxTokens: &maxTokens,
})
```
## Using a prompt template

You can apply a saved
[prompt template](/sdk/templates)to inject a system prompt and fill variables.

- Python
- Node.js
- Go

```
resp = client.chat.completions.create(
    ChatCompletionParams(
        model="openai/gpt-4o-mini",
        messages=[ChatMessage(role="user", content="Introduce yourself.")],
        template="my-template-name",
        variables={"role": "friendly pirate"},
        max_tokens=80,
        temperature=0,
    )
)
```
```
const resp = await client.chat.completions.create({
  model: "openai/gpt-4o-mini",
  messages: [{ role: "user", content: "Introduce yourself." }],
  template: "my-template-name",
  variables: { role: "friendly pirate" },
  max_tokens: 80,
  temperature: 0,
});
```
```
template := "my-template-name"
maxTokens := 80
resp, err := client.Chat.Completions.Create(ctx, meshapi.ChatCompletionParams{
    Template:  &template,
    Messages:  []meshapi.ChatMessage{{Role: "user", Content: "Greet me"}},
    MaxTokens: &maxTokens,
})
```
## Async (Python)

Use`AsyncMeshAPI` for async applications.
```
import asyncio
from meshapi import AsyncMeshAPI, ChatCompletionParams, ChatMessage
async def main():
    async with AsyncMeshAPI(base_url="https://api.meshapi.ai", token="rsk_...") as client:
        resp = await client.chat.completions.create(
            ChatCompletionParams(
                model="openai/gpt-4o-mini",
                messages=[ChatMessage(role="user", content="Say hello.")],
                max_tokens=20,
            )
        )
        print(resp.choices[0].message.content)
asyncio.run(main())
```
## Response fields

| Field | Description | 
|---|---|
| `resp.id` | Unique request ID | 
| `resp.model` | Model that served the request | 
| `resp.choices[0].message.content` | The assistant’s reply | 
| `resp.choices[0].message.role` | Always `"assistant"` | 
| `resp.choices[0].finish_reason` | `"stop"` or`"length"` | 
| `resp.usage` | Token counts: `prompt_tokens` ,`completion_tokens` ,`total_tokens` | 

## Tool calling

Pass a`tools` array to let the model call your own functions. The wire shape is
OpenAI-compatible, so the same request works across all three SDKs — see
[Tool Calling](/docs/capabilities/tool-calling)for the full request/response cycle.

- Python
- Node.js
- Go

```
from meshapi import ChatCompletionParams, ChatMessage, Tool, ToolFunction
params = ChatCompletionParams(
    model="openai/gpt-4o",
    messages=[ChatMessage(role="user", content="What is the weather in Paris?")],
    tools=[
        Tool(
            type="function",
            function=ToolFunction(
                name="get_weather",
                description="Get current weather for a city",
                parameters={
                    "type": "object",
                    "properties": {"city": {"type": "string"}},
                    "required": ["city"],
                },
            ),
        )
    ],
    tool_choice="auto",
)
for chunk in client.chat.completions.stream(params):
    delta = chunk.choices[0].delta if chunk.choices else None
    if delta and delta.tool_calls:
        print("tool call:", delta.tool_calls)
    elif delta and delta.content:
        print(delta.content, end="", flush=True)
```
```
const response = await client.chat.completions.create({
  model: "openai/gpt-4o",
  messages: [{ role: "user", content: "What is the weather in Paris?" }],
  tools: [
    {
      type: "function",
      function: {
        name: "get_weather",
        description: "Get current weather for a city",
        parameters: {
          type: "object",
          properties: { city: { type: "string" } },
          required: ["city"],
        },
      },
    },
  ],
  tool_choice: "auto",
});
console.log(response.choices[0].message.tool_calls);
```
```
response, err := client.Chat.Completions.Create(ctx, meshapi.ChatCompletionParams{
    Model:    meshapi.String("openai/gpt-4o"),
    Messages: []meshapi.ChatMessage{{Role: "user", Content: "What is the weather in Paris?"}},
    Tools: []meshapi.Tool{{
        Type: "function",
        Function: meshapi.ToolFunction{
            Name:        "get_weather",
            Description: meshapi.String("Get current weather for a city"),
            Parameters: map[string]any{
                "type":       "object",
                "properties": map[string]any{"city": map[string]any{"type": "string"}},
                "required":   []string{"city"},
            },
        },
    }},
    ToolChoice: "auto",
})
```
Any request carrying 

`tools` bypasses the [gateway response cache](/docs/capabilities/caching)— a tool call depends on live state, so replaying a stored one would be wrong. This is silent: no error, and no`X-Cache` header.

# Citations

1. Source page: https://developers.meshapi.ai/sdk/chat
