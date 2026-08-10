---
type: Web Page
title: RAG (Files & Search) - Mesh API
description: Upload documents, generate embeddings, and search with vector similarity.
resource: https://developers.meshapi.ai/sdk/rag
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

The RAG API has no DELETE endpoint — uploaded files are permanent and cannot be removed programmatically.

## Full workflow

The upload → embed → search lifecycle is 6 steps:
1

Init upload

Request a signed upload URL and get a 

`file_id`.
2

PUT to signed URL

Upload the file bytes directly to the signed URL using a plain HTTP PUT.

3

Poll upload status

Call 

`rag.get(file_id)` until `upload_status == "ready"`.
4

Embed

Call 

`rag.embed([file_id])` to start the embedding process.
5

Poll embedding status

Call 

`rag.get(file_id)` until `embedding_status == "ready"`.
6

Search

Run a semantic search query against the embedded file.

## Code examples

- Python
- Node.js
- Go

```
import time
import httpx
from meshapi import MeshAPI
from meshapi import InitUploadRequest, BulkEmbedRequest, SearchRequest
client = MeshAPI(base_url="https://api.meshapi.ai", token="rsk_...")
content = b"The quick brown fox jumps over the lazy dog."
mime_type = "text/plain"
# Step 1: Init upload
upload = client.rag.init_upload(
    InitUploadRequest(file_name="my-doc.txt", mime_type=mime_type, embed=False)
)
file_id = upload.file_id
# Step 2: PUT to signed URL
httpx.put(upload.signed_url, content=content, headers={"Content-Type": mime_type}).raise_for_status()
# Step 3: Poll upload status
deadline = time.monotonic() + 30
while time.monotonic() < deadline:
    status = client.rag.get(file_id)
    if status.upload_status == "ready":
        break
    time.sleep(2)
# Step 4: Embed
client.rag.embed(BulkEmbedRequest(file_ids=[file_id]))
# Step 5: Poll embedding status
deadline = time.monotonic() + 90
while time.monotonic() < deadline:
    status = client.rag.get(file_id)
    if status.embedding_status == "ready":
        break
    elif status.embedding_status == "failed":
        raise RuntimeError(f"embedding failed: {status.last_error_code}")
    time.sleep(3)
# Step 6: Search
results = client.rag.search(
    SearchRequest(query="fox jumps", top_k=5, file_ids=[file_id])
)
for r in results.results:
    print(f"score={r.score:.4f} text={r.text[:80]}")
```
```
const content = "The quick brown fox jumps over the lazy dog.";
const mimeType = "text/plain";
// Step 1: Init upload
const upload = await client.rag.initUpload({
  file_name: "my-doc.txt",
  mime_type: mimeType,
  embed: false,
});
const fileId = upload.file_id;
// Step 2: PUT to signed URL
await fetch(upload.signed_url, {
  method: "PUT",
  body: content,
  headers: { "Content-Type": mimeType },
});
// Step 3: Poll upload status
const uploadDeadline = Date.now() + 30_000;
while (Date.now() < uploadDeadline) {
  const s = await client.rag.get(fileId);
  if (s.upload_status === "ready") break;
  await new Promise(r => setTimeout(r, 2_000));
}
// Step 4: Embed
await client.rag.embed({ file_ids: [fileId] });
// Step 5: Poll embedding status
const embedDeadline = Date.now() + 90_000;
while (Date.now() < embedDeadline) {
  const s = await client.rag.get(fileId);
  if (s.embedding_status === "ready") break;
  if (s.embedding_status === "failed") throw new Error(`embedding failed: ${s.last_error_code}`);
  await new Promise(r => setTimeout(r, 3_000));
}
// Step 6: Search
const results = await client.rag.search({
  query: "fox jumps",
  top_k: 5,
  file_ids: [fileId],
});
results.results.forEach(r => console.log(`score=${r.score.toFixed(4)} text=${r.text.slice(0, 80)}`));
```
```
content := []byte("The quick brown fox jumps over the lazy dog.")
mimeType := "text/plain"
// Step 1: Init upload
embedFalse := false
upload, err := client.RAG.InitUpload(ctx, meshapi.InitUploadRequest{
    FileName: "my-doc.txt",
    MimeType: mimeType,
    Embed:    &embedFalse,
})
if err != nil {
    log.Fatal(err)
}
fileID := upload.FileID
// Step 2: PUT to signed URL
req, _ := http.NewRequest(http.MethodPut, upload.SignedURL, bytes.NewReader(content))
req.Header.Set("Content-Type", mimeType)
http.DefaultClient.Do(req)
// Step 3: Poll upload status
deadline := time.Now().Add(30 * time.Second)
for time.Now().Before(deadline) {
    s, _ := client.RAG.Get(ctx, fileID)
    if s.UploadStatus == "ready" {
        break
    }
    time.Sleep(2 * time.Second)
}
// Step 4: Embed
client.RAG.Embed(ctx, meshapi.BulkEmbedRequest{FileIDs: []string{fileID}})
// Step 5: Poll embedding status
deadline = time.Now().Add(90 * time.Second)
for time.Now().Before(deadline) {
    s, _ := client.RAG.Get(ctx, fileID)
    if s.EmbeddingStatus == "ready" {
        break
    }
    time.Sleep(3 * time.Second)
}
// Step 6: Search
topK := 5
results, err := client.RAG.Search(ctx, meshapi.SearchRequest{
    Query:   "fox jumps",
    TopK:    &topK,
    FileIDs: []string{fileID},
})
for _, r := range results.Results {
    fmt.Printf("score=%.4f\n", r.Score)
}
```
## List files

- Python
- Node.js
- Go

```
page = client.rag.list(limit=50, offset=0)
print(f"total={page.total}")
for f in page.files:
    print(f.file_id, f.embedding_status)
```
```
const page = await client.rag.list({ limit: 50, offset: 0 });
console.log(`total=${page.total}`);
page.files.forEach(f => console.log(f.file_id, f.embedding_status));
```
```
limit := 50
offset := 0
page, err := client.RAG.List(ctx, meshapi.ListRagFilesParams{
    Limit:  &limit,
    Offset: &offset,
})
for _, f := range page.Files {
    fmt.Println(f.FileID, f.EmbeddingStatus)
}
```
## File statuses

| Field | Values | 
|---|---|
| `upload_status` | `pending` ,`ready` ,`failed` | 
| `embedding_status` | `pending` ,`running` ,`ready` ,`failed` | 

## Search options

| Field | Type | Notes | 
|---|---|---|
| `query` | string | Plain-language question | 
| `top_k` | integer | Results to return (1–50, default 5) | 
| `file_ids` | string[] | Restrict the search to specific files | 
| `filter` | object | Match on metadata key-value pairs | 
| `date_from` | integer | Unix timestamp — only chunks created after this time | 
| `date_to` | integer | Unix timestamp — only chunks created before this time | 

## RAG chat

Combine search results with a chat completion to answer questions from your own documents.
- Python
- Node.js
- Go

```
from meshapi import ChatCompletionParams, ChatMessage, SearchRequest
results = client.rag.search(SearchRequest(query="What is the refund policy?", top_k=3))
context = "\n\n".join(r.text for r in results.results)
reply = client.chat.completions.create(
    ChatCompletionParams(
        model="openai/gpt-4o-mini",
        messages=[
            ChatMessage(role="system", content=f"Answer using only the context below.\n\n{context}"),
            ChatMessage(role="user", content="What is the refund policy?"),
        ],
    )
)
print(reply.choices[0].message.content)
```
```
const results = await client.rag.search({ query: "What is the refund policy?", top_k: 3 });
const context = results.results.map((r) => r.text).join("\n\n");
const reply = await client.chat.completions.create({
  model: "openai/gpt-4o-mini",
  messages: [
    { role: "system", content: `Answer using only the context below.\n\n${context}` },
    { role: "user", content: "What is the refund policy?" },
  ],
});
console.log(reply.choices[0].message.content);
```
```
results, err := client.RAG.Search(ctx, meshapi.SearchRequest{
    Query: "What is the refund policy?",
    TopK:  meshapi.Int(3),
})
if err != nil {
    log.Fatal(err)
}
var builder strings.Builder
for _, r := range results.Results {
    builder.WriteString(r.Text + "\n\n")
}
reply, err := client.Chat.Completions.Create(ctx, meshapi.ChatCompletionParams{
    Model: meshapi.String("openai/gpt-4o-mini"),
    Messages: []meshapi.ChatMessage{
        {Role: "system", Content: "Answer using only the context below.\n\n" + builder.String()},
        {Role: "user", Content: "What is the refund policy?"},
    },
})
```

# Citations

1. Source page: https://developers.meshapi.ai/sdk/rag
