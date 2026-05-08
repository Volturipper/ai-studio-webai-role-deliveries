# Patch Plan - ROLE_FEATURE_STORYBOARD_MULTIVIEW

Status: HARVEST_READY

## P0 - Keep as contract-first, no real run yet

Do not enable the disabled Run button in this role. The current workflow files must remain templates/candidates until workflow registry, job status, model preflight, and custom-node preflight are approved.

## P1 - Small storyboard.html polish only

Proposed touch file:

```text
scratch/experiments/storyboard-ui-20260508-133333/static/storyboard.html
```

Recommended small additive changes:

1. Rename the visible workflow label from `预留 Workflow` to `Workflow template / candidate` or Chinese equivalent: `Workflow 模板候选`.
2. Add a short helper under the dropdown: `当前仅生成 payload 草稿；模板不代表已通过 ComfyUI 预检。`
3. In `buildPayload()`, add explicit status fields:
   - `contract_version: "storyboard_multi_view.v1"`
   - `mode: "payload_preview_only"`
   - `workflow_binding_status: "template_candidate_not_preflighted"`
   - `requires_preflight: true`
   - `run_allowed: false`
4. Preserve existing `feature: "storyboard_multi_view"`, `workflow_slot`, `output_grid`, `inputs`, `params`, and `endpoints_reserved` keys for compatibility.
5. Keep the current `localStorage.ai_studio_inbox_prompt_storyboard` receive behavior.

## P1 - Do not modify shared asset code in this role

`asset-actions.js` already provides the incoming reference list used by Storyboard. Do not modify it from this feature role. Let `ROLE_ASSET_REUSE_CONTRACT` own any shared localStorage/event contract changes.

## P2 - Future backend binding contract, not this patch

Future route design after workflow registry/job API approval:

```text
POST /api/storyboard/jobs
GET /api/storyboard/jobs/{id}
GET /api/storyboard/jobs/{id}/events
```

The future POST payload should require:

- `contract_version`
- `workflow_slot` or `workflow_id`
- `references` grouped by role
- `prompt`, `angle_list`, `negative_prompt`
- `output_grid.panels` limited to 4, 9, or 25
- numeric params with min/max validation
- explicit `dry_run` or `submit` mode

Do not reuse `/api/generate` directly without a binding layer because the current storyboard page needs multi-reference upload, template resolution, preflight, and job progress semantics.

## P3 - Later UX enhancement

After cross-feature contracts settle, add drag/drop reference assignment from asset cards into `character`, `environment`, `props`, or `style` slots. Keep it additive and do not break existing file-input behavior.

## Stop conditions for Codex

Stop and report instead of patching if:

- The requested change requires raw API key handling.
- The requested change edits ComfyUI model filenames, models, or custom_nodes.
- The requested change enables real `/prompt` submission without owner approval.
- The requested change touches text-to-image or image-to-video implementation files.
- The requested change requires global installs, PATH changes, or project-external writes.
