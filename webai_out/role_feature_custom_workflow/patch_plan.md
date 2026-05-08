# Patch Plan - ROLE_FEATURE_CUSTOM_WORKFLOW

STATUS: HARVEST_READY

## Goal

Turn `static/custom-workflow.html` from a raw workflow JSON picker into a user-facing custom workflow registry, preflight, Prompt Assistant payload mapper, and prepare-only execution surface.

## Recommended merge style

Small additive patch. Avoid broad replacements.

## Phase 1: metadata contract only

Add one example registry file. Recommended path:

```text
scratch/experiments/storyboard-ui-20260508-133333/config/workflow_registry.example.json
```

Suggested schema shape:

```json
{
  "schema_version": "ai_studio.workflow_registry.v1",
  "workflows": [
    {
      "id": "zimage_text_to_image",
      "label": "Z-Image Text to Image",
      "filename": "Z-Image.json",
      "category": "text_to_image",
      "description": "Generate one image from a positive prompt.",
      "input_schema": [
        {"key": "prompt", "label": "Prompt", "type": "text", "required": true, "maps_to": [{"node_id": "23", "input": "text"}]},
        {"key": "width", "label": "Width", "type": "integer", "required": false, "default": 1024},
        {"key": "height", "label": "Height", "type": "integer", "required": false, "default": 1024},
        {"key": "seed", "label": "Seed", "type": "integer", "required": false, "default": "random"}
      ],
      "outputs": [{"type": "image", "label": "Generated image"}],
      "execution": {"enabled": false, "route": "/api/jobs", "requires_owner_confirm": true},
      "safety": {"auto_run": false, "allow_raw_workflow_id_typing": false}
    }
  ]
}
```

Do not put secrets, keys, absolute browser profile paths, or user credentials in this file.

## Phase 2: backend registry/preflight additions

Touch only workflow registry and preflight areas of `app.py`.

Add or reserve:

- `GET /api/workflows/registry`
  - returns registry entries plus current file/preflight summary.
  - includes `registered`, `unregistered`, `blockers`, `warnings`.
- `POST /api/workflows/register`
  - future additive route; accepts filename + human label + category + metadata.
  - keep disabled or local-only until reviewed if needed.
- `GET /api/workflows/{filename}/preflight`
  - keep current response compatible.
  - add fields without breaking old UI:
    - `blockers: []`
    - `warnings: []`
    - `custom_node_refs: []`
    - `required_inputs: []`
    - `can_prepare: boolean`
    - `can_execute: false` until jobs API is reviewed
    - `workflow_file_sha256`
- `POST /api/workflows/{id}/prepare`
  - optional later P1. It should validate mapping and return an execution packet preview only.
  - must not submit to ComfyUI.

Important: keep existing `GET /api/workflows` and `GET /api/workflows/{filename}/preflight` compatible so the current page does not break.

## Phase 3: frontend update

Touch only:

```text
scratch/experiments/storyboard-ui-20260508-133333/static/custom-workflow.html
```

Optional tiny helper if needed:

```text
scratch/experiments/storyboard-ui-20260508-133333/static/js/workflow-registry.js
```

Changes:

1. Replace raw-only dropdown with a workflow catalog area:
   - registered workflow card/list
   - raw filename shown as technical subtext
   - unregistered raw JSON grouped under "Unregistered"
2. Keep current IDs if possible for compatibility:
   - `workflowSelect`
   - `summary`
   - `workflowMeta`
   - `modelList`
3. Add stable IDs:
   - `workflowCatalog`
   - `workflowRequirements`
   - `workflowPayloadPanel`
   - `workflowPreparePreview`
   - `applyPromptPayloadButton`
   - `runPreflightButton`
   - `prepareExecutionButton`
4. Render text through `textContent` or an escaping helper. Avoid raw template injection for workflow names, model names, and error messages.
5. Read Prompt Assistant inbox, but do not auto-run:
   - key: `ai_studio_inbox_prompt_workflow`
   - action: apply/copy/clear only.
6. Disable real execution button until reviewed jobs API is intentionally wired.

## Phase 4: execution integration later

Do not merge job execution in the same patch unless separately reviewed.

Future execution should use the accepted `/api/jobs` design from the isolated experiment only after code review. This role recommends a prepare-only packet first:

```json
{
  "schema_version": "ai_studio.execution_packet.v1",
  "workflow_id": "zimage_text_to_image",
  "workflow_file": "Z-Image.json",
  "workflow_file_sha256": "...",
  "inputs": {"prompt": "...", "width": 1024, "height": 1024},
  "preflight": {"ok": true, "blockers": [], "warnings": []},
  "submit_route": "/api/jobs",
  "auto_submit": false
}
```

## Stop conditions for Codex

Stop and return evidence if:

- any change requires touching ComfyUI `models` or `custom_nodes`.
- any code reads or prints `.env`, token, cookie, browser profile, or API key content.
- any patch changes stable main 7860 without owner approval.
- workflow preflight cannot remain backward compatible.
- job execution requires actual `/prompt` submission before owner approval.
