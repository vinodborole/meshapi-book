---
type: Web Page
title: Models - Mesh API
description: List available models and filter by free or paid tier.
resource: https://developers.meshapi.ai/sdk/models
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## List all models

- Python
- Node.js
- Go

```
from meshapi import MeshAPI
client = MeshAPI(base_url="https://api.meshapi.ai", token="rsk_...")
models = client.models.list()
for m in models:
    print(m.id, m.name, "free" if m.is_free else "paid")
```
```
const models = await client.models.list();
models.forEach(m => {
  console.log(m.id, m.name, m.is_free ? "free" : "paid");
});
```
```
models, err := client.Models.List(ctx, meshapi.ListModelsParams{})
if err != nil {
    log.Fatal(err)
}
for _, m := range models {
    fmt.Printf("%s %s\n", m.ID, m.Name)
}
```
## List free models only

- Python
- Node.js
- Go

```
free = client.models.free()
for m in free:
    print(m.id)
```
```
const free = await client.models.free();
free.forEach(m => console.log(m.id));
```
```
free, err := client.Models.Free(ctx)
for _, m := range free {
    fmt.Println(m.ID)
}
```
## List paid models only

- Python
- Node.js
- Go

```
paid = client.models.paid()
for m in paid:
    print(m.id)
```
```
const paid = await client.models.paid();
paid.forEach(m => console.log(m.id));
```
```
paid, err := client.Models.Paid(ctx)
for _, m := range paid {
    fmt.Println(m.ID)
}
```
## Filter via list()

- Python
- Node.js
- Go

```
# Only free
filtered_free = client.models.list(free=True)
# Only paid
filtered_paid = client.models.list(free=False)
```
```
const filteredFree = await client.models.list({ free: true });
const filteredPaid = await client.models.list({ free: false });
```
```
freeTrue := true
free, _ := client.Models.List(ctx, meshapi.ListModelsParams{Free: &freeTrue})
freeFalse := false
paid, _ := client.Models.List(ctx, meshapi.ListModelsParams{Free: &freeFalse})
```
## Model fields

| Field | Description | 
|---|---|
| `id` | Model identifier used in requests (e.g. `openai/gpt-4o-mini` ) | 
| `name` | Human-readable name | 
| `is_free` | Whether the model is available on the free tier |

# Citations

1. Source page: https://developers.meshapi.ai/sdk/models
