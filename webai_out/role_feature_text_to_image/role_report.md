# ROLE_FEATURE_TEXT_TO_IMAGE Report

STATUS: HARVEST_READY
ROLE_ID: ROLE_FEATURE_TEXT_TO_IMAGE
ITERATION: v2_source_verified_contract

## Read files

- `WEB_AI_MULTI_ROLE_BASELINE_PACKET_20260509.md`
- `scratch/experiments/storyboard-ui-20260508-133333/static/zimage.html`
- `scratch/experiments/storyboard-ui-20260508-133333/static/asset-actions.js`
- `scratch/experiments/storyboard-ui-20260508-133333/app.py` relevant generation/history/config routes only
- `scratch/experiments/storyboard-ui-20260508-133333/static/js/prompt-assistant-web-adapter.js` sendTarget section only

## Baseline and boundary

Current product baseline is:

```text
D:\Codexi-studio\scratch\experiments\storyboard-ui-20260508-133333Preview: http://127.0.0.1:7861/
```

Do not treat `/static/texttext.html` as the owner-facing product entry. Do not touch stable main `7860` in this role.

## Source-verified findings

1. `static/zimage.html` is a real text-to-image surface, not a placeholder. It already contains:
   - prompt input `#prompt`;
   - dimensions `#width` and `#height`;
   - local/cloud switch `#modeLocal` / `#modeCloud`;
   - render button `#mainGenBtn`;
   - masonry history/gallery `#masonry`;
   - lightbox reuse through `applySameStyle()`;
   - history delete and queue status polling;
   - generated-card integration with `renderAssetActionButtons()` and `makeAssetDraggable()`.
2. Current Prompt Assistant receive/apply exists through localStorage key `ai_studio_inbox_textimage` and panel id `#prompt-assistant-inbox-panel`.
3. Current receive/apply behavior auto-applies only when `#prompt` is empty, and exposes one explicit `Apply` button. It does not yet expose `Append` or `Dismiss`.
4. Local generation currently posts to `/api/generate` with `prompt`, `width`, `height`, `type:"zimage"`, and `client_id`.
5. Remote/cloud generation currently posts to `/generate` and is ModelScope-specific. The product should not assume this is the final remote image API.
6. `app.py` currently exposes `/api/config/token` and the page also references `modelscope_api_token`. This role must not expand raw key behavior. Any future provider config should be backend-masked and handled by generation router/provider roles.
7. `asset-actions.js` already supports cross-page target keys for Prompt Assistant, image-to-video, image edit, upscale, and reference assets. Broad schema changes should stay with `ROLE_ASSET_REUSE_CONTRACT`.

## Product target

Text-to-image should become a product page where a non-technical user can:

- receive and review a Prompt Assistant prompt;
- apply, append, or dismiss the incoming prompt without silent overwrite;
- choose Local ComfyUI, Remote API, or future Hybrid mode with clear availability state;
- generate or reuse images as assets;
- send generated results to Prompt Assistant, image-to-video, image edit, upscale, or reference bank;
- understand why a mode is disabled or unavailable.

## Harvest decision

This role is mature enough for Codex to collect as a contract and patch-planning package. Recommended next step is a small additive `zimage.html` patch only; defer backend router and broad asset schema changes until cross-cutting roles report.
