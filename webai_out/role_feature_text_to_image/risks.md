# ROLE_FEATURE_TEXT_TO_IMAGE Risks

STATUS: HARVEST_READY
ROLE_ID: ROLE_FEATURE_TEXT_TO_IMAGE
ITERATION: v2_source_verified_contract

## P0/P1 risks

1. Remote mode is currently coupled to `/generate` and ModelScope-specific behavior. Treat it as current implementation detail, not final product architecture.
2. `zimage.html` reads a page-level `modelscope_api_token` localStorage key and can call `/api/config/token`. This role must not expand raw key behavior or print key values.
3. Local generation posts to `/api/generate`, which can submit work to ComfyUI. Real render tests require owner approval for this experiment.
4. Prompt Assistant receive/apply currently has Apply but not Append/Dismiss. Adding those is low-risk if localStorage key compatibility is preserved.
5. Asset payload shape is not yet a stable cross-feature contract. Add metadata compatibly; defer broad schema decisions to `ROLE_ASSET_REUSE_CONTRACT`.
6. CDN dependencies in `zimage.html` may make local smoke brittle if offline. This is a reliability concern but should not block the contract-first patch.

## Integration risks

1. `ROLE_GENERATION_MODE_ROUTER` may define the final provider-neutral route. Text-to-image should consume that when available rather than inventing a conflicting router.
2. `ROLE_ASSET_REUSE_CONTRACT` may define final localStorage/event payload names. Preserve backward compatibility with current keys until that contract lands.
3. `ROLE_PA_UX_LAUNCHER` owns launcher/panel visual polish. This role should not patch launcher CSS.
4. `ROLE_FEATURE_IMAGE_TO_VIDEO` and `ROLE_FEATURE_ASSET_SUMMARY_REUSE` depend on generated asset metadata; breaking current fields would cause downstream reuse failures.

## Safe defaults

- Visible disabled/reserved mode states are better than hidden modes.
- Incoming prompts must not overwrite user text silently.
- Generated card metadata should be additive and backward-compatible.
- Error states should be inline, recoverable, and secret-safe.
- Codex should stop instead of rendering if owner approval for local ComfyUI generation is missing.
