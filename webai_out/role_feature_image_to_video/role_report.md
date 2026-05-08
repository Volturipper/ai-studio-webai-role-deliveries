# ROLE_FEATURE_IMAGE_TO_VIDEO Report v2

STATUS: HARVEST_READY

## Scope
Own only the image-to-video product surface for the 7861 experiment baseline. This role does not edit text-to-image, storyboard, Prompt Assistant launcher visuals, ComfyUI directories, secrets, or stable main 7860.

## Current baseline facts
- `static/imagevideo.html` is a reserved iframe feature page, not the product entry.
- Existing query modes are `single` and `endframes`. Preserve these.
- Existing asset inbox key is `ai_studio_inbox_imagevideo`. Preserve it.
- Existing prompt inbox key is `ai_studio_inbox_prompt_imagevideo`. Preserve it.
- Existing UI currently has a simple incoming preview and Prompt Assistant prompt inbox panel, but no workflow binding, no slot assignment model, no payload preview, and no honest run/preflight state.
- Existing backend has generic workflow list/preflight/upload/generate routes. It does not yet expose an image-to-video specific capability/prepare contract.

## Product goal
Turn image-to-video from a placeholder into a truthful product page with:
1. single-reference mode;
2. first/last-frame mode;
3. Prompt Assistant prompt receive/apply;
4. generated asset reuse;
5. local ComfyUI / remote API / hybrid mode choice;
6. disabled/reserved states when workflows or remote contracts are not wired;
7. payload preview and preflight before any generation attempt.

## Main design decision
Do not wire `POST /api/generate` directly from the current page. First add an imagevideo-specific prepare/capability layer. The first safe implementation can be UI + metadata only. Real generation becomes enabled only after workflow slot mapping, preflight, required input checks, and owner-approved workflow choice are explicit.

## Codex harvest value
This v2 package is suitable for Codex collection because it is a narrow feature contract with concrete selectors, API names, smoke checks, and stop conditions.
