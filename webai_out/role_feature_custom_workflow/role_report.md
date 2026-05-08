# ROLE_FEATURE_CUSTOM_WORKFLOW Report

STATUS: HARVEST_READY
ROLE_ID: ROLE_FEATURE_CUSTOM_WORKFLOW

## Read scope

✅ Read baseline packet first.  
✅ Read `static/custom-workflow.html`.  
✅ Read only workflow/preflight/job-related areas of `app.py`.  
✅ Read only workflow registry / jobs / test-plan sections of `AI_STUDIO_NEXT_IMPLEMENTATION_PLAN.md`.

## Current state

The current custom workflow page is a useful preflight prototype, not yet a user-facing workflow product page.

What exists:

✅ Custom button label saved in `localStorage` and sent to parent via `postMessage`.  
✅ Workflow JSON dropdown populated by `GET /api/workflows`.  
✅ Model preflight via `GET /api/workflows/{filename}/preflight`.  
✅ Prompt Assistant inbox panel reads `ai_studio_inbox_prompt_workflow`.  
✅ Backend has safe filename containment for existing workflow JSON reads.  

What is missing for product use:

⬜ A real registry with human labels, categories, descriptions, required inputs, supported modes, output types, and owner-facing names.  
⬜ Avoiding raw workflow IDs/filenames as the long-term user interaction model.  
⬜ Required-input inspection beyond detected model filename references.  
⬜ Missing custom-node reporting.  
⬜ Prompt Assistant payload mapping preview and apply/clear controls.  
⬜ A prepare-only execution packet before any actual run.  
⬜ Clear `blocked / ready_to_prepare / execution_disabled / ready_to_run_after_owner_confirm` states.  

## Product direction

The page should become **Custom Workflow Registry + Preflight + Prepare Execution**, not just a raw JSON picker.

Recommended UX layout:

1. **Workflow Catalog**
   - Search/filter registered workflows.
   - Show cards with label, category, input type, output type, status chip.
   - Keep filename visible as secondary technical detail only.

2. **Workflow Detail / Requirements**
   - Human description.
   - Required inputs: prompt, reference image, first frame, last frame, width/height, seed, optional params.
   - Model requirements and found/missing state.
   - Custom node requirements and found/missing/unknown state.
   - Workflow file hash or modified time for traceability.

3. **Prompt Assistant Payload**
   - Read latest `ai_studio_inbox_prompt_workflow`.
   - Show source, timestamp, prompt, negative prompt, tags, target mode.
   - Buttons: Apply to inputs, Clear, Copy execution preview.
   - Do not auto-run after receiving payload.

4. **Preflight and Prepare**
   - Preflight checks: workflow JSON, model refs, custom node refs, required input mapping, disabled execution flag.
   - Prepare returns an execution packet preview only.
   - Actual execution should wait for reviewed `/api/jobs` merge and owner-facing confirmation.

## Contract-first recommendation

P1 should be additive and low-risk:

✅ Add registry metadata example / sidecar.  
✅ Add registry read API or enrich existing `/api/workflows`.  
✅ Extend preflight response with blockers/warnings/custom-node placeholders.  
✅ Update `custom-workflow.html` to consume registry metadata and show prepare-only state.  
⬜ Do not merge broad job execution from the separate experiment in the same patch.  

## Minimal user-facing states

- `unregistered`: raw JSON exists but lacks metadata. Show technical fallback and Register button later.
- `needs_payload`: registered workflow selected but required inputs are empty.
- `preflight_blocked`: missing model/custom node/invalid JSON/unsafe filename.
- `ready_to_prepare`: all required fields and preflight checks pass; can produce execution packet.
- `execution_disabled`: prepare packet exists but real run disabled until jobs API merge/owner confirmation.
- `ready_to_run_after_owner_confirm`: future state only after jobs API is reviewed and wired.

## Non-goals for this role

❌ Do not redesign Prompt Assistant launcher visuals.  
❌ Do not change ComfyUI `models` or `custom_nodes`.  
❌ Do not edit text-to-image, storyboard, or image-to-video implementation except future naming contract coordination.  
❌ Do not add raw API key, token, cookie, `.env`, or browser-profile behavior.  
❌ Do not treat direct `/static/custom-workflow.html` as the owner-facing product entry; use the AI Studio shell at `/`.
