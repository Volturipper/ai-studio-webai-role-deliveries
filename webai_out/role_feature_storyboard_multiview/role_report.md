# ROLE_FEATURE_STORYBOARD_MULTIVIEW Report

Status: HARVEST_READY

## Scope

This role only inspected the baseline packet and the role-listed Storyboard Multi-View files. It did not inspect secrets, API keys, cookies, browser profile contents, old backups, ComfyUI model folders, or unrelated feature implementations.

## Baseline read

Use the experiment copy as the active product baseline:

```text
D:\Codex\ai-studio\scratch\experiments\storyboard-ui-20260508-133333\
Preview: http://127.0.0.1:7861/
```

Do not present `/static/texttext.html` as the product entry; it is an iframe feature page.

## Current Storyboard Multi-View state

The page is already a usable frontend contract/prototype, not a runnable production workflow submission page.

Observed current behavior:

- Reference slots exist for `character`, `environment`, `props`, and `style`.
- Each reference slot supports local image preview and keeps up to 8 items.
- Cross-feature reference bank reads `localStorage.ai_studio_reference_assets` and can add one asset as a character reference.
- Output preview supports 4 / 9 / 25 panel grids.
- Inputs include storyboard prompt, angle/shot list, negative prompt, width, height, seed, steps, CFG, denoise, and identity lock.
- Workflow selector is currently static: `storyboard_qwen_edit_2511_api_candidate`, `2511`, `Z-Image`, `Z-Image-Enhance`, or custom id.
- Payload preview is client-side only and includes reserved endpoints for future storyboard jobs.
- Run button is intentionally disabled.
- Prompt Assistant can send storyboard draft text through `localStorage.ai_studio_inbox_prompt_storyboard` and the page applies prompt/panel list into the form.

## Product direction

Make Storyboard Multi-View a director-facing page around four concepts:

1. Reference bank: character identity, environment, props/clothing, camera/style.
2. Shot plan: prompt plus explicit angle/shot list.
3. Grid contract: 4/9/25 panels with payload preview before any run.
4. Workflow binding: future registry/job binding, with templates treated as candidates until preflight confirms models/custom nodes.

## Acceptance framing

A successful next step is not "the Run button submits to ComfyUI". A successful next step is:

- The user clearly sees the page is a storyboard planning surface.
- Workflow candidates are labeled as templates/candidates, not guaranteed runnable production workflows.
- Payload preview is stable enough for Codex/backend roles to consume later.
- Asset reuse and PA storyboard draft flows remain compatible.
- No API key behavior, ComfyUI model filenames, text-to-image, or image-to-video code is changed by this role.

## Recommended merge position

Merge this as a contract/design handoff before implementation. Let cross-cutting roles own shared asset contracts and generation mode routing before enabling real submissions.
