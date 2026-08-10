---
type: Web Page
title: Embeddings - Mesh API
description: Generate vector embeddings for text using the embeddings endpoint.
resource: https://developers.meshapi.ai/sdk/embeddings
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Generate embeddings

- Python
- Node.js
- Go

```
from meshapi import MeshAPI, EmbeddingsParams
client = MeshAPI(base_url="https://api.meshapi.ai", token="rsk_...")
result = client.embeddings.create(
    EmbeddingsParams(
        model="openai/text-embedding-3-small",
        input="MeshAPI embeddings smoke test",
    )
)
print(result.model)               # model used
print(len(result.data))           # number of embedding items
print(len(result.data[0].embedding))  # vector dimension
```
```
const result = await client.embeddings.create({
  model: "openai/text-embedding-3-small",
  input: "MeshAPI embeddings smoke test",
});
console.log(result.model);
console.log(result.data[0].embedding.length); // vector dimension
```
```
model := "openai/text-embedding-3-small"
resp, err := client.Embeddings.Create(ctx, meshapi.EmbeddingsParams{
    Model: &model,
    Input: "MeshAPI embeddings smoke test",
})
if err != nil {
    log.Fatal(err)
}
fmt.Printf("model=%s items=%d dims=%d\n", resp.Model, len(resp.Data), len(resp.Data[0].Embedding.Floats()))
```
## Batch embeddings

Pass a slice/array of strings to embed multiple inputs in a single request.
- Python
- Node.js
- Go

```
result = client.embeddings.create(
    EmbeddingsParams(
        model="openai/text-embedding-3-small",
        input=["First document", "Second document", "Third document"],
    )
)
for item in result.data:
    print(f"index={item.index} dims={len(item.embedding)}")
```
```
const result = await client.embeddings.create({
  model: "openai/text-embedding-3-small",
  input: ["First document", "Second document", "Third document"],
});
result.data.forEach(item => {
  console.log(`index=${item.index} dims=${item.embedding.length}`);
});
```
```
model := "openai/text-embedding-3-small"
resp, err := client.Embeddings.Create(ctx, meshapi.EmbeddingsParams{
    Model: &model,
    Input: []string{"First document", "Second document", "Third document"},
})
for _, item := range resp.Data {
    fmt.Printf("index=%d dims=%d\n", item.Index, len(item.Embedding.Floats()))
}
```
## Response fields

| Field | Description | 
|---|---|
| `result.data` | Array of embedding objects | 
| `result.data[i].embedding` | Float vector | 
| `result.data[i].index` | Position in the input array | 
| `result.model` | Model used | 
| `result.usage` | Token counts |

# Citations

1. Source page: https://developers.meshapi.ai/sdk/embeddings
