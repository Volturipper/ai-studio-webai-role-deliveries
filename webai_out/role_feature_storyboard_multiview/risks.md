# Risks - ROLE_FEATURE_STORYBOARD_MULTIVIEW

Status: HARVEST_READY

## P0 / must not regress

- Do not enable real storyboard run/submission until workflow registry, preflight, job API, and owner action-time approval are in place.
- Do not treat workflow template names as verified model filenames.
- Do not print, request, store, or expose API keys, tokens, cookies, `.env`, or browser profile contents.
- Do not modify ComfyUI `models` or `custom_nodes`.

## P1 / integration risks

1. Static workflow dropdown can mislead users.
   - Mitigation: label as template/candidate and add `requires_preflight` status into payload.

2. Cross-feature asset reuse is shared state.
   - Mitigation: this role should not change `asset-actions.js`; defer shared contract changes to `ROLE_ASSET_REUSE_CONTRACT`.

3. `POST /api/generate` is too generic for Storyboard Multi-View.
   - Mitigation: future backend should use a storyboard-specific adapter or job route that resolves references, workflow template, preflight, and progress before invoking any ComfyUI execution.

4. Prompt Assistant storyboard inbox is localStorage-based and first-item-only.
   - Mitigation: keep compatibility now; later add timestamp/receipt handling through cross-feature contract.

5. 25-panel grid may exceed iframe height on smaller screens.
   - Mitigation: keep preview responsive and smoke both shell and iframe sizes before owner review.

## P2 / future work

- Dynamic workflow registry binding from `GET /api/workflows`.
- Preflight display for missing model refs and custom node types.
- Drag/drop reference assignment by role.
- Durable job list and event stream once `/api/jobs` or `/api/storyboard/jobs` is approved.
