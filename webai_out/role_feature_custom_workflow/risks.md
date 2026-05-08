# Risks - ROLE_FEATURE_CUSTOM_WORKFLOW

STATUS: HARVEST_READY

## P0 / hard boundaries

- Do not read, output, or request `.env`, tokens, API keys, cookies, browser profile data, or credential files.
- Do not modify ComfyUI `models` or `custom_nodes`.
- Do not submit real ComfyUI `/prompt` from this role's proposed patch.
- Do not change stable main 7860 unless owner explicitly approves.

## P1 product risks

1. **Raw workflow filename UX**
   - Current page exposes raw JSON filenames as the primary selector.
   - Product fix: registry labels and cards first; filename as secondary technical detail.

2. **Incomplete preflight**
   - Current preflight reports model filename refs only.
   - Product fix: add blockers/warnings/custom-node placeholders and required input mapping checks.

3. **Execution path ambiguity**
   - Current app has `/api/generate`, while the plan says `/api/jobs` lives in an isolated experiment.
   - Product fix: use prepare-only packet until jobs API is reviewed and intentionally merged.

4. **Prompt Assistant payload is display-only**
   - Current page displays latest `item.prompt` from `ai_studio_inbox_prompt_workflow` but does not map it to workflow inputs.
   - Product fix: explicit apply/clear/preview controls; no auto-run.

5. **Potential HTML injection from workflow names / model names / error text**
   - Current UI builds template strings with values from filenames/model refs.
   - Product fix: render with DOM `textContent` or a small escape helper.

6. **Absolute local paths in backend response**
   - Current API returns `workflow_path` and `models_dir`.
   - Product fix: okay for local dev/debug, but owner-facing UI should avoid emphasizing full absolute paths; use basename/status unless debug details are expanded.

7. **Tailwind CDN dependency**
   - Current page uses `https://cdn.tailwindcss.com`.
   - Product fix: acceptable for prototype, but local/offline packaged UI should eventually avoid remote runtime dependency.

## Recommended fast-track

✅ Accept this role output as a contract/design packet.  
✅ Let Codex make a small additive patch only after QA role and cross-feature contracts are considered.  
✅ Keep real execution disabled until reviewed jobs API is merged.  
⬜ Visual polish can wait until contract and smoke are stable.
