---
type: Web Page
title: SDK Reference - Mesh API
description: Install the MeshAPI SDK for your language and make your first API call.
resource: https://developers.meshapi.ai/sdk/overview
timestamp: '2026-09-07T12:05:08.101958+00:00'
---

Java SDK is coming soon.

## Installation

- Python
- Node.js
- Go
- Java

`httpx` and Pydantic v2.
## Initialize the client

- Python
- Node.js
- Go

`AsyncMeshAPI`:
## Documentation Index

Fetch the complete documentation index at: [/llms.txt](/llms.txt)

Use this file to discover all available pages before exploring further.

Install the MeshAPI SDK for your language and make your first API call.

```
pip install meshapi
```
```
pip install 'meshapi[realtime]'
```
```
npm install meshapi-node-sdk
```
`npm install ws` for realtime WebSocket support; Node 22+ has WebSocket built in.```
go get github.com/aifiesta/meshapi-go-sdk
```
```
from meshapi import MeshAPI
client = MeshAPI(
    base_url="https://api.meshapi.ai",
    token="rsk_...",
)
```
```
from meshapi import AsyncMeshAPI
async with AsyncMeshAPI(base_url="https://api.meshapi.ai", token="rsk_...") as client:
    ...
```
```
import { MeshAPI } from "meshapi-node-sdk";
const client = new MeshAPI({
  baseUrl: "https://api.meshapi.ai",
  token: "rsk_...",
});
```
```
import meshapi "github.com/aifiesta/meshapi-go-sdk"
client := meshapi.New(meshapi.Config{
    BaseURL: "https://api.meshapi.ai",
    Token:   "rsk_...",
})
```
| Page | What it covers | 
|---|---|
| [Chat Completions](/sdk/chat) | Basic chat, multi-turn, templates, async | 
| [Streaming](/sdk/streaming) | Streaming responses, cancellation, recovery | 
| [Structured Output](/sdk/structured-output) | JSON schema response format | 
| [Embeddings](/sdk/embeddings) | Single and batch embeddings | 
| [Image Generation](/sdk/images) | Generate images | 
| [Audio](/sdk/audio) | Text-to-speech, speech-to-text, translate to English, list voices | 
| [Video Generation](/sdk/video) | Submit tasks, poll status, list | 
| [Batches](/sdk/batches) | Async batch job lifecycle | 
| [Model Compare](/sdk/compare) | Fan out to multiple models | 
| [Responses API](/sdk/responses) | Responses API create and stream | 
| [Prompt Templates](/sdk/templates) | Create, update, delete, use in chat | 
| [RAG (Files & Search)](/sdk/rag) | Upload, embed, and search documents | 
| [Realtime Audio](/sdk/realtime) | Bidirectional WebSocket audio sessions | 
| [Models](/sdk/models) | List models, filter free/paid | 
| [Error Handling](/sdk/error-handling) | Typed exceptions, error codes, retries |

# Citations

1. Source page: https://developers.meshapi.ai/sdk/overview
