# ROLE_FEATURE_IMAGE_TO_VIDEO Risks v2

## P0
- Accidentally wiring current page directly to `POST /api/generate` without workflow slot mapping or preflight. Mitigation: keep run gate false until explicit capabilities/prepare pass.
- Leaking provider secrets or reading `.env`/cookies/browser profile. Mitigation: imagevideo routes return metadata only and never expose raw credentials.

## P1
- Claiming remote API or hybrid mode works before contract exists. Mitigation: show disabled/reserved states with reason.
- Treating candidate workflow filenames as runnable production workflows. Mitigation: require preflight and explicit slot mapping.
- Breaking existing asset handoff by renaming `ai_studio_inbox_imagevideo`. Mitigation: preserve key and existing `handleIncomingAsset` behavior.

## P2
- UI becomes another card-heavy placeholder. Mitigation: use clear mode tabs, slots, payload preview, and one run gate.
- Prompt Assistant prompt auto-runs unexpectedly. Mitigation: prompt receive/apply is preview-only until user action.

## P3
- Later pluginization may want this as a reusable generation page module. Keep selectors/API schema stable and additive.
