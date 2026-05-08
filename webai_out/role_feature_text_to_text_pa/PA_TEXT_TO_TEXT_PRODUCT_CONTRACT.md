# PA_TEXT_TO_TEXT_PRODUCT_CONTRACT

ROLE_ID: ROLE_FEATURE_TEXT_TO_TEXT_PA

## Product surface
The text-to-text Prompt Assistant should feel like a user-facing assistant, not a developer rules console.

Entry rule:
- Owner-facing entry remains http://127.0.0.1:7861/.
- /static/texttext.html is an iframe feature page, not the main product entry.

## Functional groups
Use these visible groups while preserving existing action ids:

1. Translate
- translate_to_en: Translate to English
- translate_to_zh: Translate to Chinese

2. Prompt Polish
- expand_prompt: Expand Prompt
- optimize_prompt: Optimize Prompt
- multi_candidates: Multi-candidate Prompt

3. Image/Edit
- kontext_instruction: Kontext Edit Instruction
- qwen_edit_instruction: Qwen-Edit Instruction
- product_prompt: Product Image Prompt

4. Video/Storyboard/Workflow
- image_to_video_prompt: Image-to-Video Prompt
- storyboard_outline: Storyboard Outline
- workflow_payload: Workflow Payload

5. Asset Caption
- image_caption and video_caption remain in Caption tab or asset-triggered flows because they need an asset.

## Provider state labels
Render provider state in user language, not raw implementation terms.

States:
- demo_fallback: Demo fallback active. Configure a provider for real model output.
- ready_text: Text provider ready.
- ready_translate: Translate provider ready.
- ready_vlm: VLM provider ready.
- config_error: Provider configured but missing required base_url/model/key for non-Ollama providers.

Do not display raw API keys. Only show no key, has key, or masked key already returned by backend.

## Settings behavior
Keep provider key handling backend-only.

Minimum settings UX:
- Explain that keys are posted to backend and are not written to localStorage.
- Show active role mapping: LLM, Translate, VLM.
- If role mapping select is visible, it must either persist changes or be read-only. Do not leave a misleading editable control.
- Save/test buttons remain service-card scoped.

## Custom action behavior
The current Rules tab can remain the advanced editor, but add a normal-user path:

- Button: Create Custom Action
- Fields: Name, Group, Trigger action id, System instruction, Prompt template
- Defaults:
  - category: custom
  - action: custom
  - enabled: true
  - template includes {input}; help text explains allowed placeholders: {input}, {action}, {options}
- Save uses existing POST /api/prompt/rules.
- Advanced fields can be revealed under Advanced, not shown first.

## Output actions
Keep existing output behavior but label it clearly:
- Replace Input
- Append to Input
- Copy
- Send to Storyboard
- Send to Text-to-Image
- Send to Image-to-Video
- Send to Workflow

## Send-to-target payload
Preserve current item shape unless ROLE_ASSET_REUSE_CONTRACT later supersedes it:

```json
{
  "prompt": "string",
  "action": "string",
  "payload": {},
  "asset": null,
  "target": "storyboard|textimage|imagevideo|custom_workflow",
  "createdAt": "ISO-8601"
}
```

Storage keys to preserve:
- storyboard: ai_studio_inbox_prompt_storyboard
- textimage: ai_studio_inbox_textimage
- imagevideo: ai_studio_inbox_prompt_imagevideo
- custom_workflow: ai_studio_inbox_prompt_workflow

Event to preserve:
- prompt-assistant:send-to-target

## Non-goals for this role
- Do not redesign the compact launcher visuals.
- Do not implement receiver pages.
- Do not design the full plugin marketplace/system.
- Do not change ComfyUI directories or models.
- Do not change raw key handling.
