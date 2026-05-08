# ROLE_FEATURE_ASSET_SUMMARY_REUSE Report

STATUS: HARVEST_READY

## Scope

This is a contract-only role output for generated asset summary and cross-feature reuse in the current AI Studio experimental baseline.

Baseline:

```text
D:\Codex\ai-studio\scratch\experiments\storyboard-ui-20260508-133333\
Preview: http://127.0.0.1:7861/
```

Do not treat `/static/texttext.html` as the owner-facing product entry.

## Read files

- `WEB_AI_MULTI_ROLE_BASELINE_PACKET_20260509.md`
- `scratch/experiments/storyboard-ui-20260508-133333/static/assets-summary.html`
- `scratch/experiments/storyboard-ui-20260508-133333/static/asset-actions.js`
- `scratch/experiments/storyboard-ui-20260508-133333/static/zimage.html` asset cards only
- `scratch/experiments/storyboard-ui-20260508-133333/static/storyboard.html` reference bank only
- `scratch/experiments/storyboard-ui-20260508-133333/static/imagevideo.html` incoming asset section only

## Current behavior observed

### `assets-summary.html`

- Currently acts as a project file summary page.
- Calls `/api/assets/summary` and renders Workflows, Outputs, Pages, and Paths.
- Does not load `asset-actions.js`.
- Does not show a generated asset gallery.
- Does not expose quick reuse buttons.

### `asset-actions.js`

Already contains a useful cross-feature foundation:

- LocalStorage target keys:
  - `ai_studio_reference_assets`
  - `ai_studio_inbox_imagevideo`
  - `ai_studio_inbox_imageedit`
  - `ai_studio_inbox_upscale`
  - `ai_studio_inbox_promptassistant`
- Global helpers:
  - `window.renderAssetActionButtons(data, deleteButtonHtml)`
  - `window.makeAssetDraggable(card, data)`
  - `window.getReferenceAssets()`
  - `window.getAssetInbox(target)`
  - `window.renderIncomingAssetTray(target, options)`
- Action menu already includes REF, PA, I2V, I2I, and HD.
- Drag start writes `application/json` and `text/plain` into `dataTransfer`.

Missing pieces:

- No versioned payload contract.
- No canonical gallery key for all generated assets.
- No status/ack event beyond a parent `postMessage` with only target/count.
- No receiver-side clear/consume semantics.
- No safe escaping/sanitization contract for asset metadata before HTML rendering.

### `zimage.html` asset cards

- `renderImageCard()` calls `renderAssetActionButtons(data, deleteButton)`.
- `renderImageCard()` calls `makeAssetDraggable(card, data)`.
- This is the best current generated asset source.
- The card data shape is roughly `{ timestamp, prompt, images, type, params }`.

### `storyboard.html` reference bank

- Reads `ai_studio_reference_assets` from localStorage.
- Shows up to 8 reference assets.
- Button is currently hard-coded to `用作人物` / character role.
- It can add selected assets into the internal reference state.
- It does not yet let users route to environment / props / style roles from the bank.
- File inputs do not yet accept asset drag/drop payloads.

### `imagevideo.html` incoming asset section

- Loads `asset-actions.js`.
- Uses `renderIncomingAssetTray('imagevideo')`.
- `handleIncomingAsset(asset)` displays `#incomingPreview`, `#incomingImage`, `#incomingTitle`, and `#incomingMeta`.
- This is enough for a receive-only proof, but not enough for actual first/last-frame video execution.

## Product contract proposal

### User-facing model

The generated asset flow should feel like a persistent reuse tray:

1. User generates image assets on text-to-image or future image pages.
2. Each asset card exposes quick actions.
3. Asset Summary becomes the gallery and reuse hub.
4. Any asset can be sent to Prompt Assistant, image-to-video, storyboard reference bank, image edit, or upscale.
5. Drag-and-drop should be a convenience path, but click actions should remain the primary reliable path.

### Storage compatibility

Keep existing keys for backward compatibility. Add only one optional gallery key:

```text
ai_studio_asset_gallery_v1
```

Existing target inbox keys remain canonical for current pages:

```text
ai_studio_reference_assets
ai_studio_inbox_imagevideo
ai_studio_inbox_imageedit
ai_studio_inbox_upscale
ai_studio_inbox_promptassistant
```

### Asset payload v1

```json
{
  "contract": "ai_studio.asset_reuse.v1",
  "asset_id": "asset_<stable_slug>",
  "image": "/output/example.png",
  "images": ["/output/example.png"],
  "prompt": "source prompt or summary",
  "type": "zimage|cloud|asset|reference",
  "source_page": "zimage",
  "timestamp": 1760000000000,
  "params": {},
  "target": "reference|imagevideo|imageedit|upscale|promptassistant",
  "receivedAt": 1760000000000
}
```

### Event proposal

When an asset is sent, keep the current parent notification but also dispatch a same-page event:

```text
ai-studio-asset-target-updated
```

Event detail:

```json
{
  "contract": "ai_studio.asset_reuse.v1",
  "target": "imagevideo",
  "key": "ai_studio_inbox_imagevideo",
  "asset_id": "asset_...",
  "count": 1
}
```

### Receiver behavior

- Receivers should read the target inbox on load.
- Receivers should listen to `storage` for cross-frame updates.
- Receivers may listen to `ai-studio-asset-target-updated` for same-frame updates.
- Receivers should display a visible pending asset tray.
- Use/copy action should not automatically delete the inbox item until a later explicit consume behavior exists.
- Clear/replace semantics should be added later by `ROLE_ASSET_REUSE_CONTRACT`.

## Proposed-owned-output

This output is docs/contract only. No broad replacement code is included.

## Proposed-touch-files

- `static/assets-summary.html`
- `static/asset-actions.js`
- `static/zimage.html` asset-card wiring only if needed
- `static/storyboard.html` reference bank and drop wiring only
- `static/imagevideo.html` incoming asset section only

## Do-not-touch

- Backend provider config
- Prompt Assistant template builder
- Workflow execution internals
- ComfyUI models/custom_nodes
- Secrets, tokens, cookies, `.env`, browser profile content
- Stable main / 7860 unless owner approves

## Merge recommendation

Merge this as a feature contract after QA docs/tests and before cross-feature contracts. Implementation should be small and additive.
