# ROLE_FEATURE_TEXT_TO_IMAGE Patch Plan

STATUS: HARVEST_READY
ROLE_ID: ROLE_FEATURE_TEXT_TO_IMAGE
ITERATION: v2_source_verified_contract

## Recommended Codex merge mode

Contract-first, then one small additive UI patch. Do not rewrite the page.

## P0: Do not change these in this role

- Do not touch Prompt Assistant launcher CSS.
- Do not touch storyboard/imagevideo/custom-workflow implementations.
- Do not touch ComfyUI `models` or `custom_nodes`.
- Do not read, print, request, or embed API keys, tokens, cookies, `.env`, or browser profile data.
- Do not change stable main `7860` unless owner explicitly approves.

## P1: Minimal additive patch for `static/zimage.html`

### 1. Make incoming Prompt Assistant payload product-like

Current state: `ai_studio_inbox_textimage` is read, auto-apply happens only when `#prompt` is empty, and there is one Apply button.

Patch target:

- Preserve current localStorage key `ai_studio_inbox_textimage`.
- Keep no-silent-overwrite behavior.
- Add visible metadata in the panel:
  - source action if present;
  - createdAt if present;
  - short payload target or mode if present.
- Add buttons:
  - `Apply` replaces current prompt;
  - `Append` appends to current prompt;
  - `Dismiss` hides/removes only the current inbox item from localStorage.
- Do not auto-start generation after apply/append.

### 2. Add inline status instead of alert-only behavior

Current state: no prompt, no remote token/config, local failure, and cloud failure use `alert()` or console-only behavior.

Patch target:

- Add a compact status element near `#mainGenBtn`, for example `#zimage-run-status`.
- Route user-facing errors to this element:
  - empty prompt;
  - remote provider not configured;
  - local render failed;
  - timeout or no images returned.
- Keep console logging minimal and never log secret-bearing values.

### 3. Clarify generation modes without inventing backend

Current state: local mode uses `/api/generate`; cloud mode uses `/generate` and ModelScope-specific token flow.

Patch target:

- Keep `local` and `cloud` buttons working as they currently do.
- Rename product copy to be understandable:
  - Local ComfyUI
  - Remote API
- Add visible remote availability text:
  - `Configured` only if existing status says configured;
  - `Needs provider config` if not configured;
  - do not hide the remote mode.
- Add non-clickable/reserved `Hybrid` label only if it does not require layout churn. Otherwise document as future router scope.

### 4. Normalize asset metadata before generated cards enter asset actions

Current state: `renderImageCard(data)` passes backend/history record directly to `renderAssetActionButtons()`.

Patch target:

Before calling asset action helpers, normalize `data` to preserve at least:

```json
{
  "feature": "textimage",
  "source": "generated|history|prompt_assistant",
  "mode": "local|remote",
  "prompt": "string",
  "images": ["string"],
  "width": 1024,
  "height": 1024,
  "timestamp": 0,
  "type": "zimage|cloud|textimage",
  "params": {}
}
```

Do not rename existing fields in a breaking way.

## P1: Provider-neutral API contract for later roles only

Do not implement this route inside this role unless `ROLE_GENERATION_MODE_ROUTER` also converges on it.

```json
{
  "GET /api/generation/modes?feature=textimage": {
    "returns": {
      "feature": "textimage",
      "default_mode": "local",
      "modes": [
        {"id":"local", "label":"Local ComfyUI", "available":true, "reason":""},
        {"id":"remote", "label":"Remote API", "available":false, "reason":"provider_not_configured"},
        {"id":"hybrid", "label":"Hybrid", "available":false, "reason":"reserved"}
      ]
    }
  },
  "POST /api/generation/text-image": {
    "body": {
      "mode": "local|remote|hybrid",
      "prompt": "string",
      "width": 1024,
      "height": 1024,
      "workflow_json": "optional local workflow id",
      "params": {}
    },
    "returns": {
      "ok": true,
      "mode": "local|remote|hybrid",
      "images": ["/output/example.png"],
      "asset": {
        "feature": "textimage",
        "prompt": "string",
        "images": ["/output/example.png"],
        "width": 1024,
        "height": 1024,
        "timestamp": 0
      }
    }
  }
}
```

## Stop conditions for Codex

Stop and return evidence instead of patching if:

- the patch would require real API keys, tokens, cookies, `.env`, or browser profile content;
- the patch would change ComfyUI `models` or `custom_nodes`;
- the patch would require broad backend router changes before cross-cutting roles report;
- the patch would modify stable main `7860`;
- a real render would be required but owner has not approved local ComfyUI generation for this test.
