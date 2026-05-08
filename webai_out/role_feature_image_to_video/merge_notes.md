# Merge Notes v2

This v2 supersedes v1. Meaningful improvements:

- Anchored to the actual current `imagevideo.html`, which is only  reserved UI with existing `single/endframes` query modes.
- Preserves exact current keys: `ai_studio_inbox_imagevideo` and `ai_studio_inbox_prompt_imagevideo`.
- Narrows backend proposal to imagevideo metadata/prepare routes instead of broad generation changes.
- Adds a strict run gate and stop conditions so Codex does not prematurely call generic `/api/generate`.
- Keeps merge scope additive and role-owned.

Recommended merge order: collect this as a feature contract, wait for QA/asset-router/generation-router contracts, then implement as a small imagevideo-only patch.
