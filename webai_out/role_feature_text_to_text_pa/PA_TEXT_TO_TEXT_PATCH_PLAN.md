# PA_TEXT_TO_TEXT_PATCH_PLAN

ROLE_ID: ROLE_FEATURE_TEXT_TO_TEXT_PA
STATUS: HARVEST_READY

## Patch style
Small additive patch. Prefer JS/CSS changes first. Backend change is optional and non-secret only.

## Step 1 - Adapter helpers
File:
- scratch/experiments/storyboard-ui-20260508-133333/static/js/prompt-assistant-web-adapter.js

Add helpers near existing helper functions:
- actionGroup(action): maps category/id to one of Translate, Prompt Polish, Image/Edit, Video/Storyboard/Workflow.
- targetLabel(target): maps target id to user-facing label.
- providerStateSummary(config): returns concise text for LLM/Translate/VLM status.

Expected behavior:
- No action id changes.
- No endpoint changes.
- No raw key reads beyond existing masked config object.

## Step 2 - Group assist actions
File:
- static/js/prompt-assistant-web-adapter.js

Replace flat renderAssist action button list with grouped sections:
- Render `.pa-action-group` for each visible group.
- Render `.pa-action-group-title` before buttons.
- Keep `.pa-action-btn` buttons and onclick runAction(a.id).

Keep existing buttons for backfill, append, storyboard, textimage, imagevideo, workflow, copy. Change only labels to user-facing names if needed.

## Step 3 - Provider state clarity
File:
- static/js/prompt-assistant-web-adapter.js

Update renderPanel and updateConfigStatus:
- Keep data-role="config-status".
- Add a short state note such as `.pa-provider-note` in Settings or Header.
- Show fallback clearly when provider_configured is false after runAction.

Do not expose base URL/key/model details beyond existing settings fields and masked key.

## Step 4 - Settings role-map clarity
File:
- static/js/prompt-assistant-web-adapter.js

Minimum acceptable fix:
- Make `.pa-role-grid select[data-role-map]` read-only/disabled with helper text, because saving currently happens through service cards.

Better optional fix:
- Add an explicit Save Role Mapping button that posts full sanitized config to /api/prompt/config without touching raw keys.
- Only do this if Codex verifies raw keys are not dropped. If uncertain, choose read-only/disabled.

Recommended fast-track: disable role-grid selects and add text: "Change active role from each service card, then Save."

## Step 5 - Custom action friendly flow
File:
- static/js/prompt-assistant-web-adapter.js

Update renderRules:
- Keep existing advanced fields.
- Add a top mini flow: Create Custom Action.
- On click, prefill:
  - id: custom_<timestamp or safe label>
  - category: custom
  - action: custom
  - enabled: true
  - system: You are an AI Studio prompt helper.
  - template: Input: {input}\nTask: <user-defined task>\nOutput:
- Add help text for placeholders.

No backend change required for this minimum version.

## Step 6 - CSS support
File:
- scratch/experiments/storyboard-ui-20260508-133333/static/css/prompt-assistant-web.css

Add small CSS only:
- .pa-action-group
- .pa-action-group-title
- .pa-provider-note
- .pa-template-hint
- .pa-readonly-note

Do not edit compact launcher CSS in this role unless strictly needed to avoid broken layout.

## Step 7 - Optional backend metadata
File:
- prompt_assistant_backend.py

Only if Codex wants richer custom template metadata:
- Add optional fields to RuleUpsert: description: str = "", ui_group: str = "", display_order: int = 0, favorite: bool = False.
- Do not add secret fields.
- Do not change config/key behavior.

This is optional. The UI can work without it.

## Step 8 - Cache query
File:
- static/texttext.html

If JS/CSS changes:
- Bump only prompt-assistant-web-adapter.js and prompt-assistant-web.css cache query, e.g. v=pa-ux1.

## Stop conditions
- Any patch that prints, logs, embeds, exports, or copies raw provider keys must be rejected.
- Any patch touching zimage/imagevideo/storyboard/custom-workflow implementation must be rejected for this role.
- Any patch requiring global install, PATH change, or stable main 7860 change must stop for owner decision.
