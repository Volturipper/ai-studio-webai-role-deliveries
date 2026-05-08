# Web AI Role Harvest Summary 2026-05-09

Scope: AI Studio feature-role harvest from ChatGPT branches.

## Collected Roles

| role | status | zip | sha256 | notes |
|---|---|---|---|---|
| ROLE_FEATURE_TEXT_TO_TEXT_PA | collected | webai_out/downloads/role_feature_text_to_text_pa_harvest_ready.zip | c48c0dfb44e5b6dead3c4b799e762c5679ccaade6c44a2639436df576f2a8f5a | collected, role recommends template/asset contract review |
| ROLE_FEATURE_TEXT_TO_IMAGE | collected_needs_hash_review | webai_out/downloads/role_feature_text_to_image_handoff_v2.zip | b9d475aedf599bc263363983b6ee5733b93c2a69038c3c9cf084ed7e82d4b54d | manifest declared different zip hash and no SHA256SUMS.txt; review before applying |
| ROLE_FEATURE_STORYBOARD_MULTIVIEW | collected | webai_out/downloads/role_feature_storyboard_multiview_handoff.zip | b94627033dd0324417abcf9ebac0ef6372fc84588d165743d8d3095d70e90c9d | internal SHA256SUMS verified |
| ROLE_FEATURE_IMAGE_TO_VIDEO | collected | webai_out/downloads/role_feature_image_to_video_handoff_v2.zip | 98d255545f271afd553eebe56b1998463597a2f9fca0eef0d6dddada4f8c57a0 | internal SHA256SUMS verified |
| ROLE_FEATURE_CUSTOM_WORKFLOW | collected | webai_out/downloads/role_feature_custom_workflow_handoff.zip | f6d3706e6ab555317d12adbd4453bd4feff1e2b7661e79537f4382bdb47dfe26 | internal SHA256SUMS verified |
| ROLE_FEATURE_ASSET_SUMMARY_REUSE | collected | webai_out/downloads/role_feature_asset_summary_reuse_handoff.zip | 06d938fa10765b7a2fb9840ba2e154b3b08bac638bae265457bddbddab4c1d93 | internal SHA256SUMS verified |

## Output Folders

```text
D:\Codex\ai-studio\webai_out\role_feature_text_to_text_pa\
D:\Codex\ai-studio\webai_out\role_feature_text_to_image\
D:\Codex\ai-studio\webai_out\role_feature_storyboard_multiview\
D:\Codex\ai-studio\webai_out\role_feature_image_to_video\
D:\Codex\ai-studio\webai_out\role_feature_custom_workflow\
D:\Codex\ai-studio\webai_out\role_feature_asset_summary_reuse\
```

## Next Use

Use these as contracts and patch plans, not direct patches. The next best step is a compact integration pass that compares touch files and conflicts:

1. Custom workflow registry/preflight.
2. Asset reuse contract.
3. Storyboard frontend contract.
4. Image-to-video reserved/prepare contract.
5. Text-to-image UI contract, after hash mismatch review.
6. Prompt Assistant PA plan when text-to-text is prioritized.

Do not apply generated code blindly.
