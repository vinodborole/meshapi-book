---
type: Web Page
title: Batches | Mesh API Docs
description: Run async bulk inference jobs with the Python SDK.
resource: https://developers.meshapi.ai/sd-ks/python-sdk/batch-api
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

Python SDKBatchesAsk a question|Copy page|View as Markdown|More actionsBatches Submit a list of requests inline, kick off a batch, poll until done. Batch jobs run at discounted pricing. Results are returned inline in the poll response — no separate file download required. 1from meshapi import (2 BatchRequestItem,3 CreateBatchParams,4)56# 1. Create the batch7batch = client.batches.create(8 CreateBatchParams(9 requests=[10 BatchRequestItem(11 custom_id="req-1",12 body={13 "model": "openai/gpt-5.4",14 "messages": [{"role": "user", "content": "Say hi."}],15 },16 ),17 BatchRequestItem(18 custom_id="req-2",19 body={20 "model": "openai/gpt-5.4",21 "messages": [{"role": "user", "content": "Say bye."}],22 },23 ),24 ],25 completion_window="24h",26 )27)2829# 2. Poll later — results are included once status is "completed"30status = client.batches.get(batch.id)31if status.status == "completed":32 for result in status.results or []:33 print(result["custom_id"], result["response"]["body"]["choices"][0]["message"]["content"])

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/python-sdk/batch-api
