# ROLE_FEATURE_TEXT_TO_IMAGE Smoke Tests

STATUS: HARVEST_READY
ROLE_ID: ROLE_FEATURE_TEXT_TO_IMAGE
ITERATION: v2_source_verified_contract

These checks are designed to be low-token and safe. They do not require secrets. A real local render should run only if owner has approved ComfyUI use for this experiment.

## A. Product entry check

1. Open `http://127.0.0.1:7861/`.
2. Navigate to text-to-image through the AI Studio shell/sidebar.
3. Expected: the product shell remains visible. Do not present `/static/texttext.html` as the owner-facing product entry.

## B. Prompt Assistant receive/apply check

Browser console setup:

```js
localStorage.setItem('ai_studio_inbox_textimage', JSON.stringify([{
  prompt:'cinematic product photo of a silver necklace on white silk',
  action:'smoke',
  payload:{mode:'textimage'},
  target:'textimage',
  createdAt:new Date().toISOString()
}]));
location.reload();
```

Expected current behavior:

- `#prompt-assistant-inbox-panel` appears.
- If `#prompt` is empty, the prompt is applied.
- If `#prompt` already has user text, it is not overwritten unless the user clicks Apply.

Expected after recommended patch:

- Apply, Append, and Dismiss actions are visible.
- Apply replaces prompt; Append appends; Dismiss removes/hides current inbox item.
- No generation starts automatically.

## C. Mode selector check

1. Click local mode.
2. Expected: `zimage_engine_mode` becomes `local`; button copy communicates Local ComfyUI.
3. Click remote/cloud mode.
4. Expected: remote state is visible. If provider is not configured, show a clear configuration-needed state, not a silent failure.
5. No raw token appears in DOM, console output, or returned report.

## D. Local generation request shape, no forced render

Only run an actual `/api/generate` call if owner has approved local ComfyUI generation. Otherwise observe or mock the request.

Expected local request body:

```json
{
  "prompt": "string",
  "width": 1024,
  "height": 1024,
  "type": "zimage",
  "client_id": "non-empty"
}
```

Expected result handling:

- if `images` is non-empty, a card appears in `#masonry`;
- the card has asset actions;
- normalized metadata includes feature, mode, prompt, images, dimensions, timestamp, type, and params where available.

## E. Remote mode without provider config

1. Do not inspect secret files.
2. Use a clean browser state or only clear the non-secret page-level smoke value if necessary.
3. Select remote/cloud and click render.
4. Expected: inline config-needed state. No API key/token/cookie is printed or requested in the report.

## F. History and asset action check

1. Load `/api/history?type=zimage`.
2. Expected: `zimage` and `cloud` records with images render as cards.
3. Open one card's asset menu.
4. Expected actions include reference, Prompt Assistant, image-to-video, image edit, upscale, and delete.
5. Prompt Assistant action writes to `ai_studio_inbox_promptassistant`.
6. Image-to-video action writes to `ai_studio_inbox_imagevideo`.

## G. Lightbox reuse check

1. Open a generated/history card.
2. Click `Apply Style`.
3. Expected: prompt is copied into `#prompt`, lightbox closes, page scrolls to top, and no generation starts automatically.

## H. Console health

Expected after load and basic interactions:

- no blocking JavaScript errors;
- failed requests are limited to intentionally unavailable local ComfyUI or remote provider states;
- no secret/token/cookie output;
- CDN dependency failures, if any, are recorded separately as reliability concerns, not as text-to-image logic failures.
