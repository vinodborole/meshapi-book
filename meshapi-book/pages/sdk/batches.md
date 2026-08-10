---
type: Web Page
title: Batches - Mesh API
description: Submit async batch jobs, check status, and cancel pending batches.
resource: https://developers.meshapi.ai/sdk/batches
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

Not all models support the Batch API. 

`openai/gpt-5-nano` is confirmed to support batching.
## Create a batch

Pass an array of`requests`, each with a `custom_id` for correlating results and a `body` containing the chat completion parameters.
- Python
- Node.js
- Go

```
from meshapi import MeshAPI, CreateBatchParams, BatchRequestItem
client = MeshAPI(base_url="https://api.meshapi.ai", token="rsk_...")
batch = client.batches.create(
    CreateBatchParams(
        requests=[
            BatchRequestItem(
                custom_id="req-1",
                body={
                    "model": "openai/gpt-5-nano",
                    "messages": [{"role": "user", "content": "Reply with the single word: hello"}],
                    "max_tokens": 16,
                },
            ),
            BatchRequestItem(
                custom_id="req-2",
                body={
                    "model": "openai/gpt-5-nano",
                    "messages": [{"role": "user", "content": "Reply with the single word: world"}],
                    "max_tokens": 16,
                },
            ),
        ],
        metadata={"suite": "my-batch-job"},
    )
)
print(batch.id)
```
```
const batch = await client.batches.create({
  requests: [
    {
      custom_id: "req-1",
      body: {
        model: "openai/gpt-5-nano",
        messages: [{ role: "user", content: "Reply with the single word: hello" }],
        max_tokens: 10,
      },
    },
    {
      custom_id: "req-2",
      body: {
        model: "openai/gpt-5-nano",
        messages: [{ role: "user", content: "Reply with the single word: world" }],
        max_tokens: 10,
      },
    },
  ],
  metadata: { suite: "my-batch-job" },
});
console.log(batch.id);
```
```
batch, err := client.Batches.Create(ctx, meshapi.CreateBatchParams{
    Requests: []meshapi.BatchRequestItem{
        {
            CustomID: "req-1",
            Body: map[string]interface{}{
                "model":      "openai/gpt-5-nano",
                "messages":   []map[string]interface{}{{"role": "user", "content": "Reply with the single word: hello"}},
                "max_tokens": 10,
            },
        },
        {
            CustomID: "req-2",
            Body: map[string]interface{}{
                "model":      "openai/gpt-5-nano",
                "messages":   []map[string]interface{}{{"role": "user", "content": "Reply with the single word: world"}},
                "max_tokens": 10,
            },
        },
    },
    Metadata: map[string]interface{}{"suite": "go-batch-job"},
})
if err != nil {
    log.Fatal(err)
}
fmt.Println(batch.ID)
```
## List batches

- Python
- Node.js
- Go

```
batch_list = client.batches.list(limit=10)
for item in batch_list.data:
    print(item.id, item.status)
```
```
const list = await client.batches.list({ limit: 10 });
list.data.forEach(b => console.log(b.id, b.status));
```
```
limit := 10
listed, err := client.Batches.List(ctx, nil, &limit)
for _, item := range listed.Data {
    fmt.Println(item.ID, item.Status)
}
```
## Get a batch

- Python
- Node.js
- Go

```
got = client.batches.get(batch.id)
print(got.id, got.status)
```
```
const got = await client.batches.get(batch.id);
console.log(got.id, got.status);
```
```
got, err := client.Batches.Get(ctx, batch.ID)
fmt.Println(got.ID, got.Status)
```
## Cancel a batch

- Python
- Node.js
- Go

```
cancelled = client.batches.cancel(batch.id)
print(cancelled.id)
```
```
const cancelled = await client.batches.cancel(batch.id);
console.log(cancelled.id);
```
```
cancelled, err := client.Batches.Cancel(ctx, batch.ID)
fmt.Println(cancelled.ID, cancelled.Status)
```

# Citations

1. Source page: https://developers.meshapi.ai/sdk/batches
