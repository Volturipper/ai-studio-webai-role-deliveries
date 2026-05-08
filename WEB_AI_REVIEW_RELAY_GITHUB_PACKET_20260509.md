# AI Studio Web AI Review Relay Packet 2026-05-09

Purpose: let reviewers inspect harvested designer-role output incrementally without waiting for every designer role to finish.

Published relay:

```text
repo: https://github.com/Volturipper/ai-studio-webai-role-deliveries
branch: main
visibility: PUBLIC
```

## Read Order For Reviewers

1. `CODEX_START_HERE.md`
2. `WEB_AI_ROLE_HARVEST_SUMMARY_20260509.md`
3. `WEB_AI_ROLE_INTEGRATION_MATRIX_20260509.md`
4. Your assigned role folder(s) under `webai_out/`

## Incremental Review Model

Do not wait for all designers.

Whenever a designer role reaches `HARVEST_READY`, `DONE`, or `FILES_READY`, Codex can publish that role folder to the relay repo and notify only the relevant reviewer roles.

## Current Reviewer Routing

| designer output | primary reviewers |
|---|---|
| `webai_out/role_feature_custom_workflow/` | `ROLE_QA_AUTOMATION`, `ROLE_GENERATION_MODE_ROUTER` |
| `webai_out/role_feature_asset_summary_reuse/` | `ROLE_ASSET_REUSE_CONTRACT`, `ROLE_QA_AUTOMATION` |
| `webai_out/role_feature_storyboard_multiview/` | `ROLE_ASSET_REUSE_CONTRACT`, `ROLE_GENERATION_MODE_ROUTER`, `ROLE_QA_AUTOMATION` |
| `webai_out/role_feature_image_to_video/` | `ROLE_GENERATION_MODE_ROUTER`, `ROLE_ASSET_REUSE_CONTRACT`, `ROLE_QA_AUTOMATION` |
| `webai_out/role_feature_text_to_image/` | `ROLE_GENERATION_MODE_ROUTER`, `ROLE_QA_AUTOMATION` |
| `webai_out/role_feature_text_to_text_pa/` | `ROLE_PA_TEMPLATE_BUILDER`, `ROLE_ASSET_REUSE_CONTRACT`, `ROLE_QA_AUTOMATION` |

## Reviewer Output

Reviewers should not paste long reports into chat. They may save/package every round, but Codex harvests only when:

```text
HARVEST_READY
DONE
FILES_READY
```

Reviewer files should be under:

```text
webai_out/<review_role_id_lowercase>/
```

Recommended files:

```text
ROLE_OUTPUT_MANIFEST.json
review_report.md
conflict_matrix.md
approved_next_patch_plan.md
smoke_tests.md
risks.md
SHA256SUMS.txt
```

## Safety

- Do not ask for or output secrets.
- Do not read `.env`, tokens, cookies, API keys, or browser profile data.
- Do not propose changes to ComfyUI models/custom_nodes.
- Do not apply generated code blindly.
- Treat text-to-image harvest as `needs_hash_review` because its package lacks `SHA256SUMS.txt` and its manifest-declared zip hash differs from the actual downloaded zip hash.
