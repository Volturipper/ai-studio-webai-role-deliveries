# Web AI Role Output Protocol 2026-05-09

Purpose: keep Web AI role pages fast and make valuable role output easy to retrieve by OpenPatch/GitHub or WebAI Transfer without wasting effort on half-finished iterations.

## Rule

Roles should not paste long reports into chat.

Early and middle iterations should use compact heartbeat replies only. Roles may save or package their own work every round, but Codex should not harvest those packages until a useful milestone.

## Harvest Policy

Do not retrieve files every round.

Roles may still save files or create rolling packages every round to avoid losing work. This is allowed and often useful. The restriction is on Codex harvesting, not on role-side saving.

Use file packaging only when one of these is true:

- `HARVEST_READY`: the role says the output is close to complete and worth collecting.
- `DONE`: the role says its assigned work is thoroughly complete.
- A monitorable marker surface reports harvest-ready output.
- Codex or the owner explicitly requests a handoff package.
- The role has produced a coherent patch plan, selector/API contract, or smoke checklist that should be preserved.
- The page is becoming slow and valuable content should be moved out of chat.

For non-harvest rounds, the role reply should stay compact:

```text
STATUS: ITERATING | BLOCKED | NEEDS_OWNER_DECISION
ROLE_ID:
CHANGED:
BLOCKER:
NEXT:
```

## Required Role Files

Role-side saved files and harvest packages should use:

```text
webai_out/<role_id_lowercase>/
```

Preferred files:

```text
ROLE_OUTPUT_MANIFEST.json
role_report.md
patch_plan.md
selectors_apis.json
smoke_tests.md
risks.md
```

If the role can package files, use:

```text
<role_id_lowercase>_handoff.zip
```

## Chat Reply Limit

At harvest time, the chat reply should contain only:

```text
STATUS: HARVEST_READY | DONE | FILES_READY | NEED_TRANSFER_ROUTE | BLOCKED
ROLE_ID:
FILES:
SUMMARY:
DELIVERY_ROUTE:
```

If the role's explanation is valuable, it must be saved into a file such as `role_report.md` instead of pasted into chat.

## Delivery Routes

Preferred:

```text
OpenPatch RE -> GitHub delivery repository -> Codex reads files from repo
```

Fallback:

```text
WebAI Transfer -> bidirectional file relay -> Codex receives files under webai_out/
```

Local manual fallback:

```text
User downloads role zip -> place under D:\Codex\ai-studio\webai_out\<role_id_lowercase>\
```

## Codex Commands

List roles:

```powershell
D:\Codex\ai-studio\WEB_AI_BRANCH_TOOL.cmd --list-roles
```

Print a file-first role prompt:

```powershell
D:\Codex\ai-studio\WEB_AI_BRANCH_TOOL.cmd --role-id ROLE_FEATURE_TEXT_TO_IMAGE --print-prompt
```

Register a manually created branch URL:

```powershell
D:\Codex\ai-studio\WEB_AI_BRANCH_TOOL.cmd --role-id ROLE_FEATURE_TEXT_TO_IMAGE --source-url https://chatgpt.com/c/SOURCE --branch-url https://chatgpt.com/c/BRANCH --status manually_registered
```

Create a branch only after output transfer is stable:

```powershell
D:\Codex\ai-studio\WEB_AI_BRANCH_TOOL.cmd --source-url https://chatgpt.com/c/SOURCE --role-id ROLE_FEATURE_TEXT_TO_IMAGE --yes
```

## Safety

- Do not output or request API keys, tokens, cookies, `.env`, or browser profile contents.
- Use ASCII filenames.
- Do not ask roles to paste full code files into chat unless transfer routes fail.
- Do not create more role branches until the file delivery route has been validated.
