---
type: Web Page
title: Model Compare - Mesh API
description: Send the same prompt to multiple models in parallel and compare their
  responses.
resource: https://developers.meshapi.ai/sdk/compare
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Non-streaming compare

- Python
- Node.js
- Go

```
from meshapi import MeshAPI, CompareParams, ChatMessage
client = MeshAPI(base_url="https://api.meshapi.ai", token="rsk_...")
result = client.compare.create(
    CompareParams(
        models=["openai/gpt-4o-mini", "anthropic/claude-haiku-4.5"],
        messages=[ChatMessage(role="user", content="Explain what an API is in one sentence.")],
        skip_comparison=True,
        max_tokens=100,
    )
)
for r in result.results:
    print(r.model, r.content)
```
```
const result = await client.compare.create({
  models: ["openai/gpt-4o-mini", "anthropic/claude-haiku-4.5"],
  messages: [{ role: "user", content: "Explain what an API is in one sentence." }],
  skip_comparison: true,
  max_tokens: 100,
});
result.results.forEach(r => {
  console.log(r.model, r.content);
});
```
```
maxTokens := 100
skip := true
resp, err := client.Compare.Create(ctx, meshapi.CompareParams{
    Models: []string{"openai/gpt-4o-mini", "anthropic/claude-haiku-4.5"},
    Messages: []meshapi.ChatMessage{
        {Role: "user", Content: "Explain what an API is in one sentence."},
    },
    MaxTokens:      &maxTokens,
    SkipComparison: &skip,
})
if err != nil {
    log.Fatal(err)
}
for _, r := range resp.Results {
    fmt.Println(r.Model, *r.Content)
}
```
## Streaming compare

- Python
- Node.js
- Go

```
events = list(
    client.compare.stream(
        CompareParams(
            models=["openai/gpt-4o-mini", "anthropic/claude-haiku-4.5"],
            messages=[ChatMessage(role="user", content="Tell me a joke.")],
            skip_comparison=True,
            max_tokens=100,
        )
    )
)
print(f"received {len(events)} events")
```
```
let events = 0;
for await (const _event of client.compare.create({
  models: ["openai/gpt-4o-mini", "anthropic/claude-haiku-4.5"],
  messages: [{ role: "user", content: "Tell me a joke." }],
  skip_comparison: true,
  max_tokens: 100,
  stream: true,
})) {
  events++;
}
console.log(`received ${events} events`);
```
```
maxTokens := 100
skip := true
eventCh, errCh := client.Compare.Stream(ctx, meshapi.CompareParams{
    Models: []string{"openai/gpt-4o-mini", "anthropic/claude-haiku-4.5"},
    Messages: []meshapi.ChatMessage{
        {Role: "user", Content: "Tell me a joke."},
    },
    MaxTokens:      &maxTokens,
    SkipComparison: &skip,
})
count := 0
for range eventCh {
    count++
}
if err := <-errCh; err != nil {
    log.Fatal(err)
}
fmt.Printf("received %d events\n", count)
```
## Parameters

| Parameter | Description | 
|---|---|
| `models` | Array of model IDs to compare (2+) | 
| `messages` | Conversation messages sent to all models | 
| `skip_comparison` | Set to `true` to return raw results without a synthesis step | 
| `max_tokens` | Maximum tokens per model response |

# Citations

1. Source page: https://developers.meshapi.ai/sdk/compare
