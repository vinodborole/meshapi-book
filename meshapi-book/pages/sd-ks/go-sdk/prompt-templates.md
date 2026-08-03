---
type: Web Page
title: Prompt Templates | Mesh API Docs
description: Manage and use prompt templates with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/prompt-templates
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Prompt Templates

# Prompt Templates

Templates allow you to skip re-sending system prompts by storing them on the server.

1 // Create a template (System and Model are *string) 2 system := "You are a helpful agent for {{company}}." 3 model := "openai/gpt-4o-mini" 4 tmpl, _ := client.Templates.Create(ctx, meshapi.CreateTemplateParams{ 5 Name: "support-agent", 6 System: &system, 7 Model: &model, 8 }) 9 10 // Use it in a completion 11 resp, _ := client.Chat.Completions.Create(ctx, meshapi.ChatCompletionParams{ 12 Template: &tmpl.Name, 13 Variables: map[string]string{"company": "Acme Corp"}, 14 Messages: []meshapi.ChatMessage{{Role: "user", Content: "Hi!"}}, 15 }) 

1 // List 2 list, _ := client.Templates.List(ctx) 3 4 // Get 5 t, _ := client.Templates.Get(ctx, "uuid-or-name") 6 7 // Delete 8 _ = client.Templates.Delete(ctx, t.ID)

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/prompt-templates
