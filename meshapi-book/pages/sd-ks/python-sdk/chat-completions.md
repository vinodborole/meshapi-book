---
type: Web Page
title: Chat Completions | Mesh API Docs
description: Generate chat completions using the Python SDK.
resource: https://developers.meshapi.ai/sd-ks/python-sdk/chat-completions
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Chat Completions

# Chat Completions

1 from meshapi import MeshAPI, ChatCompletionParams, ChatMessage 2 3 client = MeshAPI(base_url="https://api.meshapi.ai", token="rsk_...") 4 5 reply = client.chat.completions.create( 6 ChatCompletionParams( 7 model="openai/gpt-4o-mini", 8 messages=[ 9 ChatMessage(role="system", content="You are a concise assistant."), 10 ChatMessage(role="user", content="What is the capital of France?"), 11 ], 12 temperature=0.7, 13 max_tokens=256, 14 ) 15 ) 16 17 print(reply.choices[0].message.content) 18 print(f"Tokens: {reply.usage.total_tokens}") 

## Async

1 import asyncio 2 from meshapi import AsyncMeshAPI, ChatCompletionParams, ChatMessage 3 4 async def main(): 5 async with AsyncMeshAPI(base_url="https://api.meshapi.ai", token="rsk_...") as client: 6 reply = await client.chat.completions.create( 7 ChatCompletionParams( 8 model="openai/gpt-4o-mini", 9 messages=[ChatMessage(role="user", content="Hello!")], 10 ) 11 ) 12 print(reply.choices[0].message.content) 13 14 asyncio.run(main()) 

## Streaming

`stream()` is a separate method from `create()`. It returns an iterator (sync) or async iterator (async) of chunks.

1 for chunk in client.chat.completions.stream( 2 ChatCompletionParams( 3 model="openai/gpt-4o-mini", 4 messages=[ChatMessage(role="user", content="Write a haiku about Python.")], 5 ) 6 ): 7 if chunk.choices and chunk.choices[0].delta: 8 print(chunk.choices[0].delta.content or "", end="", flush=True) 

Async streaming:

1 async for chunk in client.chat.completions.stream(params): 2 if chunk.choices and chunk.choices[0].delta: 3 print(chunk.choices[0].delta.content or "", end="", flush=True) 

## Tool calling

1 from meshapi import ChatCompletionParams, ChatMessage, Tool, ToolFunction 2 3 params = ChatCompletionParams( 4 model="openai/gpt-4o", 5 messages=[ChatMessage(role="user", content="What is the weather in Paris?")], 6 tools=[ 7 Tool( 8 type="function", 9 function=ToolFunction( 10 name="get_weather", 11 description="Get current weather for a city", 12 parameters={ 13 "type": "object", 14 "properties": {"city": {"type": "string"}}, 15 "required": ["city"], 16 }, 17 ), 18 ) 19 ], 20 tool_choice="auto", 21 ) 22 23 for chunk in client.chat.completions.stream(params): 24 delta = chunk.choices[0].delta if chunk.choices else None 25 if delta and delta.tool_calls: 26 print("tool call:", delta.tool_calls) 27 elif delta and delta.content: 28 print(delta.content, end="", flush=True)

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/python-sdk/chat-completions
