# PA_TEXT_TO_TEXT_SMOKE_CHECKLIST

ROLE_ID: ROLE_FEATURE_TEXT_TO_TEXT_PA

## Pre-check
- Use product shell: http://127.0.0.1:7861/
- Do not present /static/texttext.html as the owner-facing entry.
- Do not read or print .env, cookies, tokens, API keys, browser profiles, or credential files.

## Static check
- Confirm only allowed files changed for this role.
- Confirm no raw key literal or credential content was added to JS/CSS/HTML/docs.
- Confirm stable main 7860 was not modified.

## UI smoke
1. Open http://127.0.0.1:7861/.
2. Navigate to text-to-text/Prompt Assistant feature inside shell.
3. Confirm #promptAssistantPanel renders.
4. Confirm quick actions are grouped and all buttons remain clickable.
5. Confirm settings explains fallback vs provider ready.
6. Confirm role-map UI is not misleading: either read-only with helper text or saved explicitly.
7. Confirm Rules tab offers a friendly Create Custom Action path or prefill behavior.
8. Confirm History tab can restore a previous output.

## API smoke without secrets
Use a harmless prompt such as: "a cinematic product shot of a silver bracelet".

- GET /api/prompt/actions returns ok and action ids.
- POST /api/prompt/assist with expand_prompt returns ok.
- If no provider is configured, source should be local_demo_fallback and UI should say demo fallback.
- GET /api/prompt/config returns has_api_key/api_key_masked only, not raw api_key.
- POST /api/prompt/rules can create one custom rule with no secret fields.
- GET /api/prompt/history returns the latest action.

## Send target smoke
After a generated output:
- Click Send to Storyboard. Check ai_studio_inbox_prompt_storyboard gets a new item.
- Click Send to Text-to-Image. Check ai_studio_inbox_textimage gets a new item.
- Click Send to Image-to-Video. Check ai_studio_inbox_prompt_imagevideo gets a new item.
- Click Send to Workflow. Check ai_studio_inbox_prompt_workflow gets a new item.

Expected item fields:
- prompt
- action
- payload
- asset
- target
- createdAt

## Browser console smoke
- No blocking console errors on page load.
- No raw key/token/cookie/env/browser profile content in DOM or console.

## Pass condition
Pass if grouped actions, provider/fallback clarity, custom action prefill, history restore, and send-to-target behavior work without exposing secrets or changing out-of-scope pages.
