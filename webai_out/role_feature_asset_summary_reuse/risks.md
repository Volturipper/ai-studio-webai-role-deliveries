# ROLE_FEATURE_ASSET_SUMMARY_REUSE Risks

STATUS: HARVEST_READY

## P0/P1 risks to handle before broad UI expansion

### R1 - Unescaped asset metadata in HTML

Current asset and reference rendering uses HTML template strings. If localStorage contains malformed or hostile text, prompts/types/images could be rendered unsafely.

Mitigation:

- Add small escaping helpers.
- Prefer `textContent` for prompt/type labels.
- Validate image URL forms before assigning to `src`.

### R2 - localStorage bloat

Generated images may include large data URLs. Repeatedly writing many assets can exceed localStorage limits.

Mitigation:

- De-dupe by image URL / asset_id.
- Cap gallery and target inbox lengths.
- Avoid copying duplicate large strings across every target when possible.

### R3 - Target semantics are not fully stable

`reference`, `imagevideo`, `imageedit`, `upscale`, and `promptassistant` exist, but receivers are uneven.

Mitigation:

- Keep current keys for compatibility.
- Add `contract: ai_studio.asset_reuse.v1`.
- Keep click actions as primary path.
- Treat `imageedit` and `upscale` as reserved unless receiver pages exist.

### R4 - Storyboard currently assumes character reference

The storyboard reference bank button is hard-coded to `用作人物`.

Mitigation:

- Add role buttons: character, environment, props, style.
- Do not change storyboard execution or workflow binding.

### R5 - Image-to-video receive path is UI-only

`imagevideo.html` can display an incoming reference, but actual video workflow execution is not wired in this role.

Mitigation:

- Label it clearly as received reference / reserved execution.
- Leave video execution to `ROLE_FEATURE_IMAGE_TO_VIDEO` and generation router roles.

### R6 - Overlap with cross-cutting roles

This role designs product behavior for the asset summary/reuse feature. `ROLE_ASSET_REUSE_CONTRACT` may later formalize cross-page contracts.

Mitigation:

- Keep this output feature-owned and additive.
- Do not edit unrelated pages or provider config.
- Hand off payload/event/key proposal to the cross-cutting role.

## Non-goals

- No backend provider config changes.
- No Prompt Assistant template builder changes.
- No workflow execution internals.
- No ComfyUI model/custom node changes.
- No secrets, tokens, cookies, `.env`, or browser profile content.
- No stable main / 7860 changes unless owner approves.
