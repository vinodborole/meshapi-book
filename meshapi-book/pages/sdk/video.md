---
type: Web Page
title: Video Generation - Mesh API
description: Submit video generation tasks, poll for completion, and list past generations.
resource: https://developers.meshapi.ai/sdk/video
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Generate a video

- Python
- Node.js
- Go

```
from meshapi import MeshAPI, VideoGenerationParams, VideoContentItem
client = MeshAPI(base_url="https://api.meshapi.ai", token="rsk_...")
resp = client.videos.generate(
    VideoGenerationParams(
        model="byteplus/dreamina-seedance-2-0",
        content=[VideoContentItem(type="text", text="A serene mountain lake at sunrise")],
    )
)
print(resp.id)      # task ID
print(resp.status)  # initial status
```
```
const resp = await client.videos.generate({
  model: "byteplus/dreamina-seedance-2-0",
  content: [{ type: "text", text: "A serene mountain lake at sunrise" }],
});
console.log(resp.id);     // task ID
console.log(resp.status); // initial status
```
```
text := "A serene mountain lake at sunrise"
resp, err := client.Videos.Generate(ctx, meshapi.VideoGenerationParams{
    Model: "byteplus/dreamina-seedance-2-0",
    Content: []meshapi.VideoContentItem{
        {Type: "text", Text: &text},
    },
})
if err != nil {
    log.Fatal(err)
}
fmt.Printf("task_id=%s\n", resp.ID)
```
## Retrieve task status

- Python
- Node.js
- Go

```
task = client.videos.retrieve(resp.id)
print(task.id)
print(task.status)  # pending / running / succeeded / failed
```
```
const task = await client.videos.retrieve(resp.id);
console.log(task.id);
console.log(task.status); // pending / running / succeeded / failed
```
```
task, err := client.Videos.Retrieve(ctx, resp.ID)
fmt.Printf("status=%s\n", task.Status)
```
## Poll until complete

```
import time
while True:
    task = client.videos.retrieve(resp.id)
    if task.status == "succeeded":
        print("done:", task.id)
        break
    elif task.status == "failed":
        raise RuntimeError(f"video generation failed: {task.id}")
    time.sleep(5)
```
## List past generations

- Python
- Node.js
- Go

```
from meshapi import ListVideoGenerationsParams
listing = client.videos.list(ListVideoGenerationsParams(limit=5))
print(f"total={listing.total} items={len(listing.data)}")
for item in listing.data:
    print(item.id, item.status)
```
```
const listing = await client.videos.list({ limit: 5 });
console.log(`total=${listing.total} items=${listing.data.length}`);
```
```
limit := 5
listing, err := client.Videos.List(ctx, &meshapi.ListVideoGenerationsParams{
    Limit: &limit,
})
fmt.Printf("total=%d items=%d\n", listing.Total, len(listing.Data))
```
## Task statuses

| Status | Meaning | 
|---|---|
| `pending` | Task queued, not yet started | 
| `running` | Generation in progress | 
| `succeeded` | Generation complete | 
| `failed` | Generation failed |

# Citations

1. Source page: https://developers.meshapi.ai/sdk/video
