# ROLE_FEATURE_ASSET_SUMMARY_REUSE Smoke Tests

STATUS: HARVEST_READY

These are low-token acceptance checks for Codex after a future small implementation patch. They do not require API keys, secrets, provider calls, or ComfyUI execution.

## Preconditions

- Use current experimental baseline only:

```text
D:\Codex\ai-studio\scratch\experiments\storyboard-ui-20260508-133333\
http://127.0.0.1:7861/
```

- Do not test owner-facing product behavior through `/static/texttext.html`.
- Do not read `.env`, tokens, cookies, or browser profile content.
- Do not submit ComfyUI jobs for this role.

## Smoke 1 - Asset actions still appear on text-to-image cards

1. Open product shell at `http://127.0.0.1:7861/`.
2. Navigate to the text-to-image page through the shell/sidebar.
3. Ensure at least one image card is visible from history or test fixture.
4. Hover/focus the card.
5. Verify the action menu exists and includes REF, PA, I2V, I2I, HD.
6. Verify no console error is introduced by `asset-actions.js`.

Expected: PASS.

## Smoke 2 - Send to image-to-video inbox

1. From a text-to-image asset card, click `I2V`.
2. Navigate to image-to-video page through the shell/sidebar.
3. Verify an incoming tray appears.
4. Click `Use here`.
5. Verify `#incomingPreview` becomes visible and `#incomingImage` is populated.

Expected localStorage key: `ai_studio_inbox_imagevideo`.
Expected: PASS.

## Smoke 3 - Send to storyboard reference bank

1. From a text-to-image asset card, click `REF`.
2. Navigate to storyboard page through the shell/sidebar.
3. Verify `#reference-bank` becomes visible.
4. Click one reference-role action.
5. Verify the corresponding `.ref-slot[data-role="..."] .ref-gallery` gains a thumbnail.

Expected localStorage key: `ai_studio_reference_assets`.
Expected: PASS.

## Smoke 4 - Asset Summary gallery

1. Navigate to the Asset Summary page through the shell/sidebar if available, or load its frame route through the current shell.
2. Verify original `/api/assets/summary` sections still render or fallback gracefully.
3. Verify generated asset gallery section renders from localStorage.
4. Verify quick actions on gallery cards use the same action menu as text-to-image cards.

Expected proposed gallery key: `ai_studio_asset_gallery_v1`.
Expected: PASS.

## Smoke 5 - Same-origin no-secret check

1. Search changed role files for suspicious strings: `.env`, `cookie`, `token`, `api_key`, `Authorization`.
2. Confirm no new code reads, prints, or sends credential content.
3. Confirm no patch touches backend provider config.

Expected: PASS.

## Smoke 6 - Metadata escaping check

1. Insert a localStorage test asset with prompt text containing angle brackets.
2. Render the asset gallery/reference bank/incoming tray.
3. Verify the prompt is displayed as text and does not execute as HTML.

Example test prompt: `<asset smoke should render as text>`.
Expected: PASS.

## Smoke 7 - Drag/drop contract, future optional

1. Drag an asset card from text-to-image to a storyboard `.drop` zone.
2. Verify the storyboard slot adds a reference thumbnail.
3. Verify no file upload, backend call, or ComfyUI submission is triggered.

Expected: PASS when drag/drop patch exists; SKIP until implemented.

## Minimal evidence Codex should return

```text
role=ROLE_FEATURE_ASSET_SUMMARY_REUSE
entry=http://127.0.0.1:7861/
changed_files=<list>
smoke_1_asset_actions=pass|fail|skip
smoke_2_i2v_inbox=pass|fail|skip
smoke_3_storyboard_reference=pass|fail|skip
smoke_4_asset_summary_gallery=pass|fail|skip
smoke_5_no_secret=pass|fail|skip
smoke_6_escape=pass|fail|skip
smoke_7_dragdrop=pass|fail|skip
console_errors=<summary>
notes=<short>
```
