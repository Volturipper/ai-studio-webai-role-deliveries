# ROLE_FEATURE_ASSET_SUMMARY_REUSE Patch Plan

STATUS: HARVEST_READY
PATCH_TYPE: contract-only; no implementation diff included

## Goal

Turn current asset summary/reuse primitives into a product-grade generated asset reuse feature without touching provider config or workflow execution internals.

## P0 - Contract and safety before UI expansion

### P0.1 Add a versioned asset payload contract

Recommended location: `static/asset-actions.js` comments/constants or later a small docs file.

Keep existing keys:

```text
ai_studio_reference_assets
ai_studio_inbox_imagevideo
ai_studio_inbox_imageedit
ai_studio_inbox_upscale
ai_studio_inbox_promptassistant
```

Add optional gallery key:

```text
ai_studio_asset_gallery_v1
```

Asset payload should include:

```text
contract, asset_id, image, images, prompt, type, source_page, timestamp, params, target, receivedAt
```

### P0.2 Escape metadata before rendering

Current code renders asset metadata through template strings in several places. Before expanding this feature, add small helpers in `asset-actions.js`:

```text
escapeHtml(value)
safeImageUrl(value)
normalizeAsset(data)
```

Rules:

- Render prompts/types with text-safe escaping or DOM `textContent`.
- Accept only expected image URL forms: relative local paths, blob:, data:image/*, http:, https:.
- Do not render arbitrary HTML from localStorage.

### P0.3 Cap localStorage size

Add caps:

```text
max_gallery_items: 200
max_target_inbox_items: 80
max_prompt_chars_in_card: 140
max_prompt_chars_in_panel: 900
```

Avoid storing very large duplicate data URLs repeatedly.

## P1 - Make Asset Summary the reuse hub

### P1.1 Load asset action helper

In `static/assets-summary.html`, add:

```html
<script src="/static/asset-actions.js?v=6"></script>
```

### P1.2 Add gallery sections

Recommended sections:

```text
Generated Assets
Reference Inbox
Image-to-video Inbox
Prompt Assistant Inbox
Reserved: Image Edit / Upscale
```

### P1.3 Render quick reuse buttons

For each gallery card, use the existing helper:

```text
window.renderAssetActionButtons(asset)
```

This keeps UX consistent with `zimage.html`.

### P1.4 Keep existing `/api/assets/summary` list

Do not remove current Workflows / Outputs / Pages summary. The new asset gallery should be additive.

## P1 - Source page wiring

### P1.5 Register generated cards into gallery

In `asset-actions.js`, make `registerAsset(data)` upsert into `ai_studio_asset_gallery_v1` after normalization.

Expected effect:

- Loading `zimage.html` history cards makes them visible in the Asset Summary gallery.
- Duplicate image paths should de-dupe by `image` or `asset_id`.

Do not change generation execution.

## P1 - Receiver page wiring

### P1.6 Storyboard reference bank role routing

In `static/storyboard.html` reference bank only:

- Replace single hard-coded `用作人物` button with role buttons:
  - character
  - environment
  - props
  - style
- Continue writing to existing `state.references[role]`.
- Keep max 8 per role.

### P1.7 Drag-and-drop contract for storyboard slots

In `.ref-slot .drop` only:

- On `dragover`, prevent default and show active state.
- On `drop`, parse `application/json` asset payload.
- If payload has `image`, call `addReferences(role, [asset])`.
- Do not upload or submit anything.

### P1.8 Image-to-video receive behavior

In `static/imagevideo.html` incoming asset section only:

- Keep `renderIncomingAssetTray('imagevideo')`.
- Display clear text for current mode:
  - `single`: use as single image reference.
  - `endframes`: reserved; needs first/last frame assignment by image-to-video role.
- Do not implement actual video run here.

## P2 - Future enhancements, not current merge blockers

- Add clear/consume buttons per target inbox.
- Add target badges and last-used timestamps.
- Add drag ghost/visual drop target polish.
- Add search/filter by prompt, type, source page.
- Add asset selection multi-send.

## Stop conditions

Stop and return evidence if:

- Any patch needs backend provider config.
- Any patch needs workflow execution internals.
- Any patch needs ComfyUI models/custom_nodes.
- Any patch attempts to read or output secrets, tokens, cookies, `.env`, or browser profile content.
- UI changes require stable main / 7860 instead of the current 7861 experimental baseline.

## Codex merge order hint

This role output should be collected as a feature contract first. Implementation can be split later:

1. `asset-actions.js` contract/safety/gallery helpers.
2. `assets-summary.html` gallery UI.
3. `storyboard.html` reference-bank role buttons and drop handling.
4. `imagevideo.html` incoming panel labels only.
