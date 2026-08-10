---
type: Web Page
title: Chat Compare - Mesh API
resource: https://developers.meshapi.ai/api/chat/chat-compare
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

```
curl --request POST \
  --url https://api.meshapi.ai/v1/chat/compare \
  --header 'Authorization: Bearer <token>' \
  --header 'Content-Type: application/json' \
  --data '
{
  "models": [
    "openai/gpt-4o-mini",
    "anthropic/claude-haiku-4.5"
  ],
  "messages": [
    {
      "role": "user",
      "content": "Explain TCP vs UDP in two sentences."
    }
  ],
  "comparison_model": "openai/gpt-4o-mini"
}
'
```
```
import requests
url = "https://api.meshapi.ai/v1/chat/compare"
payload = {
    "models": ["openai/gpt-4o-mini", "anthropic/claude-haiku-4.5"],
    "messages": [
        {
            "role": "user",
            "content": "Explain TCP vs UDP in two sentences."
        }
    ],
    "comparison_model": "openai/gpt-4o-mini"
}
headers = {
    "Authorization": "Bearer <token>",
    "Content-Type": "application/json"
}
response = requests.post(url, json=payload, headers=headers)
print(response.text)
```
```
const options = {
  method: 'POST',
  headers: {Authorization: 'Bearer <token>', 'Content-Type': 'application/json'},
  body: JSON.stringify({
    models: ['openai/gpt-4o-mini', 'anthropic/claude-haiku-4.5'],
    messages: [{role: 'user', content: 'Explain TCP vs UDP in two sentences.'}],
    comparison_model: 'openai/gpt-4o-mini'
  })
};
fetch('https://api.meshapi.ai/v1/chat/compare', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```
```
<?php
$curl = curl_init();
curl_setopt_array($curl, [
  CURLOPT_URL => "https://api.meshapi.ai/v1/chat/compare",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => json_encode([
    'models' => [
        'openai/gpt-4o-mini',
        'anthropic/claude-haiku-4.5'
    ],
    'messages' => [
        [
                'role' => 'user',
                'content' => 'Explain TCP vs UDP in two sentences.'
        ]
    ],
    'comparison_model' => 'openai/gpt-4o-mini'
  ]),
  CURLOPT_HTTPHEADER => [
    "Authorization: Bearer <token>",
    "Content-Type: application/json"
  ],
]);
$response = curl_exec($curl);
$err = curl_error($curl);
curl_close($curl);
if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```
```
package main
import (
	"fmt"
	"strings"
	"net/http"
	"io"
)
func main() {
	url := "https://api.meshapi.ai/v1/chat/compare"
	payload := strings.NewReader("{\n  \"models\": [\n    \"openai/gpt-4o-mini\",\n    \"anthropic/claude-haiku-4.5\"\n  ],\n  \"messages\": [\n    {\n      \"role\": \"user\",\n      \"content\": \"Explain TCP vs UDP in two sentences.\"\n    }\n  ],\n  \"comparison_model\": \"openai/gpt-4o-mini\"\n}")
	req, _ := http.NewRequest("POST", url, payload)
	req.Header.Add("Authorization", "Bearer <token>")
	req.Header.Add("Content-Type", "application/json")
	res, _ := http.DefaultClient.Do(req)
	defer res.Body.Close()
	body, _ := io.ReadAll(res.Body)
	fmt.Println(string(body))
}
```
```
HttpResponse<String> response = Unirest.post("https://api.meshapi.ai/v1/chat/compare")
  .header("Authorization", "Bearer <token>")
  .header("Content-Type", "application/json")
  .body("{\n  \"models\": [\n    \"openai/gpt-4o-mini\",\n    \"anthropic/claude-haiku-4.5\"\n  ],\n  \"messages\": [\n    {\n      \"role\": \"user\",\n      \"content\": \"Explain TCP vs UDP in two sentences.\"\n    }\n  ],\n  \"comparison_model\": \"openai/gpt-4o-mini\"\n}")
  .asString();
```
```
require 'uri'
require 'net/http'
url = URI("https://api.meshapi.ai/v1/chat/compare")
http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
request = Net::HTTP::Post.new(url)
request["Authorization"] = 'Bearer <token>'
request["Content-Type"] = 'application/json'
request.body = "{\n  \"models\": [\n    \"openai/gpt-4o-mini\",\n    \"anthropic/claude-haiku-4.5\"\n  ],\n  \"messages\": [\n    {\n      \"role\": \"user\",\n      \"content\": \"Explain TCP vs UDP in two sentences.\"\n    }\n  ],\n  \"comparison_model\": \"openai/gpt-4o-mini\"\n}"
response = http.request(request)
puts response.read_body
```
```
{
  "comparison_id": "cmp_01ARZ3NDEKTSV4RRFFQ6",
  "object": "compare.completion",
  "created": 1748331628,
  "models": [
    "openai/gpt-4o-mini",
    "anthropic/claude-haiku-4.5",
    "google/gemini-3-flash-preview"
  ],
  "results": [
    {
      "model": "openai/gpt-4o-mini",
      "response_body": {
        "id": "chatcmpl-DrTNOwFsT3v79HmQwOwMgEM2IbWJl",
        "object": "chat.completion",
        "created": 1748331628,
        "model": "gpt-4o-mini-2024-07-18",
        "choices": [
          {
            "index": 0,
            "message": {
              "role": "assistant",
              "content": "TCP is a connection-oriented protocol that guarantees ordered, reliable delivery; UDP is connectionless and sends datagrams without delivery or ordering guarantees."
            },
            "finish_reason": "stop"
          }
        ],
        "usage": {
          "prompt_tokens": 24,
          "completion_tokens": 47,
          "total_tokens": 71
        }
      },
      "content": "TCP is a connection-oriented protocol that guarantees ordered, reliable delivery; UDP is connectionless and sends datagrams without delivery or ordering guarantees.",
      "latency_ms": 812,
      "usage": {
        "prompt_tokens": 24,
        "completion_tokens": 47,
        "total_tokens": 71
      },
      "request_id": "req_01ARZ3NDEKTSV4RRFFQ69G5FAV::openai/gpt-4o-mini"
    },
    {
      "model": "anthropic/claude-haiku-4.5",
      "response_body": {
        "id": "chatcmpl-019ed1d24b0371a390ab",
        "object": "chat.completion",
        "model": "anthropic/claude-haiku-4.5",
        "choices": [
          {
            "index": 0,
            "message": {
              "role": "assistant",
              "content": "TCP establishes a connection and guarantees ordered, intact delivery. UDP fires packets without a connection, trading reliability for lower latency."
            },
            "finish_reason": "stop"
          }
        ],
        "usage": {
          "prompt_tokens": 24,
          "completion_tokens": 52,
          "total_tokens": 76
        }
      },
      "content": "TCP establishes a connection and guarantees ordered, intact delivery. UDP fires packets without a connection, trading reliability for lower latency.",
      "latency_ms": 650,
      "usage": {
        "prompt_tokens": 24,
        "completion_tokens": 52,
        "total_tokens": 76
      },
      "request_id": "req_01ARZ3NDEKTSV4RRFFQ69G5FAV::anthropic/claude-haiku-4.5"
    },
    {
      "model": "google/gemini-3-flash-preview",
      "latency_ms": 0,
      "error": "Upstream provider returned an error.",
      "error_code": "upstream_error",
      "request_id": "req_01ARZ3NDEKTSV4RRFFQ69G5FAV::google/gemini-3-flash-preview"
    }
  ],
  "comparison": "Both answers are correct. Claude's phrasing is slightly clearer on the latency trade-off; GPT-4o-mini is more precise on ordering guarantees.",
  "comparison_model": "openai/gpt-4o-mini",
  "comparison_usage": {
    "prompt_tokens": 180,
    "completion_tokens": 60,
    "total_tokens": 240
  },
  "comparison_fallback_used": false,
  "total_latency_ms": 1340,
  "partial": true,
  "skip_comparison": false
}
```
# Chat Compare

```
curl --request POST \
  --url https://api.meshapi.ai/v1/chat/compare \
  --header 'Authorization: Bearer <token>' \
  --header 'Content-Type: application/json' \
  --data '
{
  "models": [
    "openai/gpt-4o-mini",
    "anthropic/claude-haiku-4.5"
  ],
  "messages": [
    {
      "role": "user",
      "content": "Explain TCP vs UDP in two sentences."
    }
  ],
  "comparison_model": "openai/gpt-4o-mini"
}
'
```
```
import requests
url = "https://api.meshapi.ai/v1/chat/compare"
payload = {
    "models": ["openai/gpt-4o-mini", "anthropic/claude-haiku-4.5"],
    "messages": [
        {
            "role": "user",
            "content": "Explain TCP vs UDP in two sentences."
        }
    ],
    "comparison_model": "openai/gpt-4o-mini"
}
headers = {
    "Authorization": "Bearer <token>",
    "Content-Type": "application/json"
}
response = requests.post(url, json=payload, headers=headers)
print(response.text)
```
```
const options = {
  method: 'POST',
  headers: {Authorization: 'Bearer <token>', 'Content-Type': 'application/json'},
  body: JSON.stringify({
    models: ['openai/gpt-4o-mini', 'anthropic/claude-haiku-4.5'],
    messages: [{role: 'user', content: 'Explain TCP vs UDP in two sentences.'}],
    comparison_model: 'openai/gpt-4o-mini'
  })
};
fetch('https://api.meshapi.ai/v1/chat/compare', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```
```
<?php
$curl = curl_init();
curl_setopt_array($curl, [
  CURLOPT_URL => "https://api.meshapi.ai/v1/chat/compare",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => json_encode([
    'models' => [
        'openai/gpt-4o-mini',
        'anthropic/claude-haiku-4.5'
    ],
    'messages' => [
        [
                'role' => 'user',
                'content' => 'Explain TCP vs UDP in two sentences.'
        ]
    ],
    'comparison_model' => 'openai/gpt-4o-mini'
  ]),
  CURLOPT_HTTPHEADER => [
    "Authorization: Bearer <token>",
    "Content-Type: application/json"
  ],
]);
$response = curl_exec($curl);
$err = curl_error($curl);
curl_close($curl);
if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```
```
package main
import (
	"fmt"
	"strings"
	"net/http"
	"io"
)
func main() {
	url := "https://api.meshapi.ai/v1/chat/compare"
	payload := strings.NewReader("{\n  \"models\": [\n    \"openai/gpt-4o-mini\",\n    \"anthropic/claude-haiku-4.5\"\n  ],\n  \"messages\": [\n    {\n      \"role\": \"user\",\n      \"content\": \"Explain TCP vs UDP in two sentences.\"\n    }\n  ],\n  \"comparison_model\": \"openai/gpt-4o-mini\"\n}")
	req, _ := http.NewRequest("POST", url, payload)
	req.Header.Add("Authorization", "Bearer <token>")
	req.Header.Add("Content-Type", "application/json")
	res, _ := http.DefaultClient.Do(req)
	defer res.Body.Close()
	body, _ := io.ReadAll(res.Body)
	fmt.Println(string(body))
}
```
```
HttpResponse<String> response = Unirest.post("https://api.meshapi.ai/v1/chat/compare")
  .header("Authorization", "Bearer <token>")
  .header("Content-Type", "application/json")
  .body("{\n  \"models\": [\n    \"openai/gpt-4o-mini\",\n    \"anthropic/claude-haiku-4.5\"\n  ],\n  \"messages\": [\n    {\n      \"role\": \"user\",\n      \"content\": \"Explain TCP vs UDP in two sentences.\"\n    }\n  ],\n  \"comparison_model\": \"openai/gpt-4o-mini\"\n}")
  .asString();
```
```
require 'uri'
require 'net/http'
url = URI("https://api.meshapi.ai/v1/chat/compare")
http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
request = Net::HTTP::Post.new(url)
request["Authorization"] = 'Bearer <token>'
request["Content-Type"] = 'application/json'
request.body = "{\n  \"models\": [\n    \"openai/gpt-4o-mini\",\n    \"anthropic/claude-haiku-4.5\"\n  ],\n  \"messages\": [\n    {\n      \"role\": \"user\",\n      \"content\": \"Explain TCP vs UDP in two sentences.\"\n    }\n  ],\n  \"comparison_model\": \"openai/gpt-4o-mini\"\n}"
response = http.request(request)
puts response.read_body
```
```
{
  "comparison_id": "cmp_01ARZ3NDEKTSV4RRFFQ6",
  "object": "compare.completion",
  "created": 1748331628,
  "models": [
    "openai/gpt-4o-mini",
    "anthropic/claude-haiku-4.5",
    "google/gemini-3-flash-preview"
  ],
  "results": [
    {
      "model": "openai/gpt-4o-mini",
      "response_body": {
        "id": "chatcmpl-DrTNOwFsT3v79HmQwOwMgEM2IbWJl",
        "object": "chat.completion",
        "created": 1748331628,
        "model": "gpt-4o-mini-2024-07-18",
        "choices": [
          {
            "index": 0,
            "message": {
              "role": "assistant",
              "content": "TCP is a connection-oriented protocol that guarantees ordered, reliable delivery; UDP is connectionless and sends datagrams without delivery or ordering guarantees."
            },
            "finish_reason": "stop"
          }
        ],
        "usage": {
          "prompt_tokens": 24,
          "completion_tokens": 47,
          "total_tokens": 71
        }
      },
      "content": "TCP is a connection-oriented protocol that guarantees ordered, reliable delivery; UDP is connectionless and sends datagrams without delivery or ordering guarantees.",
      "latency_ms": 812,
      "usage": {
        "prompt_tokens": 24,
        "completion_tokens": 47,
        "total_tokens": 71
      },
      "request_id": "req_01ARZ3NDEKTSV4RRFFQ69G5FAV::openai/gpt-4o-mini"
    },
    {
      "model": "anthropic/claude-haiku-4.5",
      "response_body": {
        "id": "chatcmpl-019ed1d24b0371a390ab",
        "object": "chat.completion",
        "model": "anthropic/claude-haiku-4.5",
        "choices": [
          {
            "index": 0,
            "message": {
              "role": "assistant",
              "content": "TCP establishes a connection and guarantees ordered, intact delivery. UDP fires packets without a connection, trading reliability for lower latency."
            },
            "finish_reason": "stop"
          }
        ],
        "usage": {
          "prompt_tokens": 24,
          "completion_tokens": 52,
          "total_tokens": 76
        }
      },
      "content": "TCP establishes a connection and guarantees ordered, intact delivery. UDP fires packets without a connection, trading reliability for lower latency.",
      "latency_ms": 650,
      "usage": {
        "prompt_tokens": 24,
        "completion_tokens": 52,
        "total_tokens": 76
      },
      "request_id": "req_01ARZ3NDEKTSV4RRFFQ69G5FAV::anthropic/claude-haiku-4.5"
    },
    {
      "model": "google/gemini-3-flash-preview",
      "latency_ms": 0,
      "error": "Upstream provider returned an error.",
      "error_code": "upstream_error",
      "request_id": "req_01ARZ3NDEKTSV4RRFFQ69G5FAV::google/gemini-3-flash-preview"
    }
  ],
  "comparison": "Both answers are correct. Claude's phrasing is slightly clearer on the latency trade-off; GPT-4o-mini is more precise on ordering guarantees.",
  "comparison_model": "openai/gpt-4o-mini",
  "comparison_usage": {
    "prompt_tokens": 180,
    "completion_tokens": 60,
    "total_tokens": 240
  },
  "comparison_fallback_used": false,
  "total_latency_ms": 1340,
  "partial": true,
  "skip_comparison": false
}
```
#### Authorizations

Enter your MeshAPI key (`rsk_...`) — sent as `Authorization: Bearer <key>`.

#### Body

`1 - 10` elements
## Show child attributes

Show child attributes

## Show child attributes

Show child attributes

`0 <= x <= 2``x >= 1`
## Show child attributes

Show child attributes

#### Response

Per-model results plus an optional synthesized comparison (JSON), or an SSE stream when stream=true

Unique ID for this comparison (`cmp_...`).

Unix timestamp (seconds) when the response was produced.

Models that were compared, in request order (deduped).

Per-model results, in `models` order.

## Show child attributes

Show child attributes

End-to-end latency for the whole compare request, in milliseconds.

`"compare.completion"`
Synthesized evaluation of all responses from the comparison LLM. Null when `skip_comparison` is true or fewer than two models succeeded.

Model that produced `comparison`. Null when no synthesis ran.

Token usage for the comparison LLM call.

## Show child attributes

Show child attributes

True if the primary comparison model failed and a fallback produced the synthesis.

True if at least one model in `results` returned an error.

Echoes whether the comparison LLM step was skipped.

# Citations

1. Source page: https://developers.meshapi.ai/api/chat/chat-compare
