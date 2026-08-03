---
type: Web Page
title: Prompt Templates | Mesh API Docs
description: Manage and use prompt templates with the Python SDK.
resource: https://developers.meshapi.ai/sd-ks/python-sdk/prompt-templates
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Prompt Templates

# Prompt Templates

Server-stored prompts with `{{variable}}` interpolation. Reference them by name from `chat.completions` to skip re-sending system prompts every request.

1 from meshapi import MeshAPI, CreateTemplateParams, ChatCompletionParams, ChatMessage 2 3 # Manage templates with your API key 4 client = MeshAPI(base_url="https://api.meshapi.ai", token="rsk_...") 5 6 client.templates.create( 7 CreateTemplateParams( 8 name="support-agent", 9 system="You are a support agent for {{company}}. Be concise and friendly.", 10 model="openai/gpt-4o-mini", 11 variables=["company"], 12 ) 13 ) 14 15 reply = client.chat.completions.create( 16 ChatCompletionParams( 17 messages=[ChatMessage(role="user", content="How do I reset my password?")], 18 template="support-agent", 19 variables={"company": "Acme Corp"}, 20 ) 21 ) 

1 from meshapi import UpdateTemplateParams 2 3 templates = client.templates.list() 4 t = client.templates.get(template_id) 5 client.templates.update(template_id, UpdateTemplateParams(model="openai/gpt-4o")) 6 client.templates.delete(template_id)

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/python-sdk/prompt-templates
