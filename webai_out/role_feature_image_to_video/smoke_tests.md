# ROLE_FEATURE_IMAGE_TO_VIDEO Smoke Tests v2

## API smoke, no generation

Run after patch from experiment root only. Do not provide secrets.

1. `GET /api/imagevideo/capabilities`
   - expect JSON schema_version `ai_studio.imagevideo.capabilities.v1`;
   - expect `status` either `reserved` or `ready`;
   - expect no raw keys/tokens/cookies;
   - expect no ComfyUI `/prompt` submission.

2. `POST /api/imagevideo/prepare` with empty payload
   - expect `ok=false`;
   - expect `run_allowed=false`;
   - expect clear `blocked_reasons`.

3. `POST /api/imagevideo/prepare` with single mode and one fake local asset reference
   - expect normalized payload preview;
   - expect `run_allowed=false` unless workflow/preflight/slot mapping are explicitly available.

## Browser smoke

Open product shell: `http://127.0.0.1:7861/`. Do not use `/static/imagevideo.html` as owner-facing entry.

Check imagevideo iframe/page:
- mode single renders title/primary slot;
- mode endframes renders first/last slots;
- incoming asset from `ai_studio_inbox_imagevideo` appears without auto-run;
- Prompt Assistant item from `ai_studio_inbox_prompt_imagevideo` appears in prompt preview without auto-run;
- local/remote/hybrid cards show clear availability/disabled reasons;
- run button is disabled or blocked with inline reason until gates pass;
- no blocking console errors.

## Evidence return

Return only:
- changed files list;
- API response summaries;
- browser check summary;
- console error count;
- whether any ComfyUI `/prompt` was submitted, expected `false`;
- SHA256 of changed files or patch zip.
