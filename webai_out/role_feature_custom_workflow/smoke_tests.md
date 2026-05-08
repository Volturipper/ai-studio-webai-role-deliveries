# Smoke Tests - ROLE_FEATURE_CUSTOM_WORKFLOW

STATUS: HARVEST_READY

Run only after a concrete custom-workflow patch is applied to the experiment copy.

## Safe backend checks

```text
cd /d D:\Codexi-studio\scratch\experiments\storyboard-ui-20260508-133333
python -m py_compile app.py
```

```text
GET http://127.0.0.1:7861/
GET http://127.0.0.1:7861/static/custom-workflow.html
GET http://127.0.0.1:7861/api/workflows
GET http://127.0.0.1:7861/api/workflows/Z-Image.json/preflight
```

Expected:

✅ `/` and `/static/custom-workflow.html` return 200.  
✅ `/api/workflows` returns at least the known workflow files.  
✅ `/api/workflows/Z-Image.json/preflight` remains backward compatible.  
✅ Added registry fields do not remove current fields.  

## UI smoke through product shell

Open:

```text
http://127.0.0.1:7861/
```

Expected:

✅ Navigate to the custom workflow entry inside the AI Studio shell, not by presenting `/static/custom-workflow.html` as the owner-facing product.  
✅ Workflow list/card area appears.  
✅ Selecting a workflow updates requirement/preflight panels.  
✅ Prompt Assistant inbox panel can show a received prompt payload if `ai_studio_inbox_prompt_workflow` exists.  
✅ Apply payload fills mapped preview fields, not auto-run.  
✅ Prepare button is disabled when blockers exist.  
✅ No blocking console errors.  

## Compatibility smoke

Before and after patch, these current selectors should still exist unless the patch intentionally aliases them:

```text
#labelInput
#workflowSelect
#summary
#workflowMeta
#modelList
```

Expected:

✅ Current custom nav label behavior still works.  
✅ Existing preflight dropdown path still works as fallback.  

## Negative tests

```text
GET /api/workflows/..%2Fsecret.json/preflight
GET /api/workflows/not-a-json.txt/preflight
GET /api/workflows/missing.json/preflight
```

Expected:

✅ path traversal rejected.  
✅ non-json rejected.  
✅ missing workflow returns controlled error.  

## No-go tests

Do not run as part of this role smoke:

❌ No ComfyUI `/prompt` submission.  
❌ No model downloads.  
❌ No ComfyUI `models` or `custom_nodes` modification.  
❌ No reading/printing `.env`, tokens, API keys, cookies, browser profile files.  
❌ No broad import of external workflow templates.
