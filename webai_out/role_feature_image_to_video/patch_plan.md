# ROLE_FEATURE_IMAGE_TO_VIDEO Patch Plan v2

STATUS: HARVEST_READY

## Patch 1 - imagevideo.html UI contract

Target: `scratch/experiments/storyboard-ui-20260508-133333/static/imagevideo.html`

Keep current URL modes:
- `/static/imagevideo.html`
- `/static/imagevideo.html?mode=single`
- `/static/imagevideo.html?mode=endframes`

Keep current inbox keys:
- `ai_studio_inbox_imagevideo`
- `ai_studio_inbox_prompt_imagevideo`

Add stable regions without replacing the whole page:
- `#i2v-status-pill`
- `#i2v-mode-tabs`
- `#i2v-generation-mode`
- `#i2v-local-mode`
- `#i2v-remote-mode`
- `#i2v-hybrid-mode`
- `#i2v-reference-slot-primary`
- `#i2v-reference-slot-first`
- `#i2v-reference-slot-last`
- `#i2v-prompt-preview`
- `#i2v-apply-prompt`
- `#i2v-workflow-select`
- `#i2v-preflight-status`
- `#i2v-payload-preview`
- `#i2v-run-button`
- `#i2v-result-panel`

Default state:
- status pill says `reserved`;
- run button disabled;
- payload preview visible/read-only;
- local/remote/hybrid cards visible with clear reasons;
- no automatic generation on asset or prompt receive.

Slot assignment rules:
- `single`: incoming image becomes primary reference.
- `endframes`: incoming image becomes first frame by default. UI must allow `Assign as last frame`.
- prompt inbox fills preview only. It must not auto-run.

## Patch 2 - backend metadata routes

Target: `scratch/experiments/storyboard-ui-20260508-133333/app.py`

Add these routes only if small and isolated. They must not read secrets and must not call ComfyUI `/prompt`.

### `GET /api/imagevideo/capabilities`
Return capability metadata. Safe default:

```json
{
  "schema_version": "ai_studio.imagevideo.capabilities.v1",
  "ok": true,
  "status": "reserved",
  "modes": {
    "single": {"available": false, "reason": "workflow_slot_mapping_not_bound"},
    "endframes": {"available": false, "reason": "workflow_slot_mapping_not_bound"}
  },
  "generation_modes": {
    "local_comfy": {"available": false, "reason": "workflow_not_selected_or_preflight_not_ok"},
    "remote_api": {"available": false, "reason": "remote_i2v_contract_not_bound"},
    "hybrid": {"available": false, "reason": "requires_local_and_remote_contracts"}
  },
  "candidate_workflows": [],
  "required_inputs": ["prompt", "reference_image"],
  "supports": ["asset_inbox", "prompt_inbox", "payload_preview", "preflight"]
}
```

Codex may reuse existing `/api/workflows` to populate candidate filenames only. Do not claim a workflow is runnable without `/api/workflows/{filename}/preflight` and explicit slot mapping.

### `POST /api/imagevideo/prepare`
Validate a UI payload and return normalized preview. It must not submit jobs.

Required response fields:
- `schema_version`
- `ok`
- `status`
- `blocked_reasons`
- `normalized_payload`
- `run_allowed`

`run_allowed` must be false unless all required image slots are present, prompt requirement is satisfied, generation mode is available, workflow is selected, and preflight passed.

## Patch 3 - blocked generate stub only if useful

Optional: `POST /api/imagevideo/generate` may exist as a blocked stub returning 409 or JSON `{ok:false,status:"blocked"}`. Do not connect it to generic `/api/generate` yet.

## Patch 4 - cache bump only

Target: `static/index.html`, imagevideo frame entries only.

If imagevideo assets are cached, bump the iframe URL query for imagevideo only. Do not alter sidebar structure.

## Stop conditions
Stop and return evidence if:
- implementation needs real ComfyUI workflow node mapping;
- implementation needs remote API credentials or provider details;
- changes would touch text-to-image/storyboard/Prompt Assistant launcher;
- changes would modify ComfyUI models/custom_nodes or stable main 7860.
