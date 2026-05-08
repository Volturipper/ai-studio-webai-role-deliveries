# AI Studio Review Relay Start Here

This repository is a Web AI review relay for AI Studio harvested role outputs.

## Scope

Review harvested role contracts and patch plans. Do not apply code directly.

## Start

Read:

```text
WEB_AI_REVIEW_RELAY_GITHUB_PACKET_20260509.md
WEB_AI_ROLE_HARVEST_SUMMARY_20260509.md
WEB_AI_ROLE_INTEGRATION_MATRIX_20260509.md
```

Then read only the role folders assigned to your reviewer role.

## Role Folders

```text
webai_out/role_feature_text_to_text_pa/
webai_out/role_feature_text_to_image/
webai_out/role_feature_storyboard_multiview/
webai_out/role_feature_image_to_video/
webai_out/role_feature_custom_workflow/
webai_out/role_feature_asset_summary_reuse/
```

## Output Rule

Use compact chat status only. Save durable review output as files.

Normal iteration:

```text
STATUS: ITERATING | BLOCKED | NEEDS_OWNER_DECISION
ROLE_ID:
CHANGED:
BLOCKER:
NEXT:
```

Harvest:

```text
STATUS: HARVEST_READY | DONE | FILES_READY
ROLE_ID:
FILES:
SUMMARY:
DELIVERY_ROUTE:
```
