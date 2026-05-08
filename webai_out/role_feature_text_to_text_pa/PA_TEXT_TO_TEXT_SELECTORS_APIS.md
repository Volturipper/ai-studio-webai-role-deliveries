# PA_TEXT_TO_TEXT_SELECTORS_APIS

ROLE_ID: ROLE_FEATURE_TEXT_TO_TEXT_PA

## Selectors currently owned or observed
Touch allowed:
- #ideaInput
- #promptAssistantPanel
- #promptAssistantLauncher
- #storyboardDraft only for existing sendToStoryboard preview behavior
- .pa-tabs
- [data-tab]
- [data-content="assist"]
- [data-content="settings"]
- [data-content="rules"]
- [data-content="history"]
- [data-content="caption"]
- [data-role="config-status"]
- [data-role="rule-select"]
- [data-role-map]
- [data-x="save-service"]
- [data-x="test-service"]
- [data-x="add-service"]
- [data-x="save-rule"]
- [data-x="reset-rules"]
- [data-x="load-rule"]
- [data-x="del-rule"]
- [data-x="backfill"]
- [data-x="append"]
- [data-x="storyboard"]
- [data-x="textimage"]
- [data-x="imagevideo"]
- [data-x="workflow"]
- [data-x="copy"]

Do not change selectors relied on by other roles unless adding backward-compatible aliases.

## APIs observed
- GET /api/prompt/actions
- POST /api/prompt/assist
- GET /api/prompt/config
- POST /api/prompt/config
- POST /api/prompt/config/service
- POST /api/prompt/config/test
- GET /api/prompt/rules
- POST /api/prompt/rules
- DELETE /api/prompt/rules/{rule_id}
- POST /api/prompt/rules/reset
- GET /api/prompt/tags
- POST /api/prompt/tags
- DELETE /api/prompt/tags/{tag_id}
- POST /api/prompt/tags/reset
- GET /api/prompt/history
- DELETE /api/prompt/history
- POST /api/prompt/caption-image
- POST /api/prompt/caption-video
- GET /api/prompt/destinations

## Storage/event contracts observed
Incoming Prompt Assistant asset inbox:
- ai_studio_inbox_promptassistant

Prompt output target keys:
- ai_studio_inbox_prompt_storyboard
- ai_studio_inbox_textimage
- ai_studio_inbox_prompt_imagevideo
- ai_studio_inbox_prompt_workflow

Events:
- prompt-assistant:send-to-target
- prompt-assistant:storyboard

## Backend safety expectations
- /api/prompt/config must return safe_config only.
- safe_config must not include raw api_key.
- Service cards may accept password input, but raw key must only be sent to backend on save/test and never written to localStorage.
