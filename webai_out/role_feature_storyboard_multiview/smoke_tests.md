# Smoke Tests - ROLE_FEATURE_STORYBOARD_MULTIVIEW

Status: HARVEST_READY

These are low-token checks Codex can run after applying any small Storyboard-only UI polish. They should not submit to ComfyUI.

## Static/API smoke

```text
cd /d D:\Codexi-studio\scratch\experiments\storyboard-ui-20260508-133333
python -m py_compile app.py
```

With the experiment server already running on 7861:

```text
GET http://127.0.0.1:7861/
GET http://127.0.0.1:7861/static/storyboard.html
GET http://127.0.0.1:7861/api/workflows
```

Expected:

- `/` returns the shell page.
- `/static/storyboard.html` returns 200.
- `/api/workflows` returns JSON.
- No request is made to ComfyUI `/prompt` by merely loading storyboard.

## Browser smoke

At `http://127.0.0.1:7861/`:

1. Open the Storyboard Multi-View sidebar entry / iframe.
2. Confirm selectors exist:
   - `#reference-list`
   - `.ref-slot[data-role="character"]`
   - `.ref-slot[data-role="environment"]`
   - `.ref-slot[data-role="props"]`
   - `.ref-slot[data-role="style"]`
   - `#grid-mode`
   - `#story-prompt`
   - `#angle-list`
   - `#workflow-slot`
   - `#build-payload`
   - `#payload-preview`
3. Click 4/9/25 grid buttons and confirm `#grid-summary` changes.
4. Enter prompt, angle list, negative prompt, width/height, and slider values.
5. Click `生成请求草稿`.
6. Confirm payload JSON includes:
   - `feature: storyboard_multi_view`
   - selected `workflow_slot`
   - grouped `references`
   - `output_grid.panels`
   - `submit_enabled: false`
7. Confirm the Run button remains disabled.

## Prompt Assistant inbox smoke

In the Storyboard iframe context or via browser dev console, set:

```javascript
localStorage.setItem('ai_studio_inbox_prompt_storyboard', JSON.stringify([
  { prompt: '测试分镜：同一人物在同一场景做四个镜头。', payload: { panels: [{shot:'front'}, {shot:'side'}, {shot:'back'}, {shot:'close-up'}] } }
]));
window.dispatchEvent(new StorageEvent('storage', { key: 'ai_studio_inbox_prompt_storyboard' }));
```

Expected:

- `#story-prompt` receives the prompt if empty.
- `#angle-list` receives numbered panel lines if empty.
- `#prompt-assistant-storyboard-inbox` appears.
- Reapply button can force apply the latest inbox item.

## Asset reference smoke

In the Storyboard iframe context or via browser dev console, set:

```javascript
localStorage.setItem('ai_studio_reference_assets', JSON.stringify([
  { image: '/static/example-placeholder.png', prompt: 'asset reference smoke', type: 'generated' }
]));
window.dispatchEvent(new StorageEvent('storage', { key: 'ai_studio_reference_assets' }));
```

Expected:

- `#reference-bank` becomes visible.
- Clicking `用作人物` adds the asset into the character reference slot.
- Payload preview includes the reference under `inputs.references.character`.

## Must not happen

- No raw API key appears in DOM, payload preview, console output, or localStorage from this role.
- No real ComfyUI `/prompt` submission is triggered.
- No changes are made to ComfyUI `models` or `custom_nodes`.
- No edits are made to stable main 7860 unless owner explicitly approves.
