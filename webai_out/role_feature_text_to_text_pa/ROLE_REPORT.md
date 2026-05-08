# ROLE_REPORT

ROLE_ID: ROLE_FEATURE_TEXT_TO_TEXT_PA
STATUS: HARVEST_READY

## read-files
- WEB_AI_MULTI_ROLE_BASELINE_PACKET_20260509.md
- scratch/experiments/storyboard-ui-20260508-133333/static/texttext.html
- scratch/experiments/storyboard-ui-20260508-133333/static/js/prompt-assistant-web-adapter.js
- scratch/experiments/storyboard-ui-20260508-133333/static/css/prompt-assistant-web.css
- scratch/experiments/storyboard-ui-20260508-133333/static/css/components/prompt-assistant-compact-launcher.css
- scratch/experiments/storyboard-ui-20260508-133333/prompt_assistant_backend.py

## scope
Productize the text-to-text Prompt Assistant surface only: quick actions, custom template creation, settings clarity, history, send-to-target behavior, and real-provider/fallback states.

## findings
- Current page mounts PromptAssistantWebAdapter from static/texttext.html into #promptAssistantPanel and #promptAssistantLauncher.
- Existing adapter already covers actions, settings, rules, tags, history, caption, output controls, and send-to-target localStorage handoff.
- Existing backend already keeps provider key handling backend-only in normal config responses by returning has_api_key and api_key_masked.
- Main product gaps are user-facing clarity: action grouping, fallback/real-provider state labels, target display names, and a non-developer custom action flow.
- The active role selector grid is visible, but role changes are saved only through individual service cards; this can confuse users.

## proposed-owned-output
- webai_out/role_feature_text_to_text_pa/ROLE_REPORT.md
- webai_out/role_feature_text_to_text_pa/PA_TEXT_TO_TEXT_PRODUCT_CONTRACT.md
- webai_out/role_feature_text_to_text_pa/PA_TEXT_TO_TEXT_PATCH_PLAN.md
- webai_out/role_feature_text_to_text_pa/PA_TEXT_TO_TEXT_SELECTORS_APIS.md
- webai_out/role_feature_text_to_text_pa/PA_TEXT_TO_TEXT_SMOKE_CHECKLIST.md
- webai_out/role_feature_text_to_text_pa/manifest.json
- webai_out/role_feature_text_to_text_pa/HARVEST_READY.marker

## proposed-touch-files
- scratch/experiments/storyboard-ui-20260508-133333/static/js/prompt-assistant-web-adapter.js
- scratch/experiments/storyboard-ui-20260508-133333/static/css/prompt-assistant-web.css
- scratch/experiments/storyboard-ui-20260508-133333/static/texttext.html only for cache query bump if JS/CSS changes
- scratch/experiments/storyboard-ui-20260508-133333/prompt_assistant_backend.py only if Codex chooses optional non-secret rule metadata fields

## do-not-touch
- zimage/imagevideo/storyboard/custom-workflow implementation
- ComfyUI paths
- ComfyUI models/custom_nodes
- raw key storage/output
- .env, tokens, API keys, cookies, browser profiles, credential files
- stable main 7860 unless owner explicitly approves
- broad launcher visual redesign; leave to ROLE_PA_UX_LAUNCHER

## decision
HARVEST_READY for Codex collection as a role-owned contract and patch plan. No independent review required before collection. Recommended downstream review role after collection: ROLE_PA_TEMPLATE_BUILDER for custom template flow overlap, and ROLE_ASSET_REUSE_CONTRACT for send-target contract overlap.
