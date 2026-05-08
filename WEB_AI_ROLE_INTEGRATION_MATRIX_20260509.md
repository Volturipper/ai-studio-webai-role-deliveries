# Web AI Role Integration Matrix 2026-05-09

Source: harvested feature-role packages under `D:\Codex\ai-studio\webai_out\`.

## Integration Order

1. `ROLE_FEATURE_CUSTOM_WORKFLOW`
   - Reason: defines registry/preflight and prepare-only execution contracts used by other feature pages.
   - Touches: `static/custom-workflow.html`, optional `app.py` registry/preflight additions, optional workflow registry metadata/helper.

2. `ROLE_FEATURE_ASSET_SUMMARY_REUSE`
   - Reason: defines shared asset gallery, target inboxes, reuse buttons, reference bank, and future drag/drop contract.
   - Touches: `static/assets-summary.html`, `static/asset-actions.js`, targeted receive/wiring in `zimage.html`, `storyboard.html`, `imagevideo.html`.

3. `ROLE_FEATURE_STORYBOARD_MULTIVIEW`
   - Reason: frontend contract for 4/9/25 storyboard reference grids; should depend on workflow registry before real execution.
   - Touches: `static/storyboard.html` only for small additive UI contract polish now.

4. `ROLE_FEATURE_IMAGE_TO_VIDEO`
   - Reason: baseline is reserved-only; should truthfully expose capability and prepare contracts before generation.
   - Touches: `static/imagevideo.html`, optional `app.py` metadata/prepare route, optional `static/index.html` cache bump.

5. `ROLE_FEATURE_TEXT_TO_IMAGE`
   - Reason: useful UI contract, but package integrity has a zip hash mismatch and no `SHA256SUMS.txt`.
   - Touches: `static/zimage.html`, optional `static/asset-actions.js`, optional future `app.py`.
   - Gate: review package hash mismatch before applying.

6. `ROLE_FEATURE_TEXT_TO_TEXT_PA`
   - Reason: Prompt Assistant contract is collected; apply when text-to-text is prioritized.
   - Touches: `static/js/prompt-assistant-web-adapter.js`, `static/css/prompt-assistant-web.css`, optional `texttext.html` cache bump, optional non-secret backend metadata.

## Cross-Role Review Needed

Create or use these review roles now:

- `ROLE_QA_AUTOMATION`: verify smoke coverage, selectors, and no accidental real generation/submission.
- `ROLE_ASSET_REUSE_CONTRACT`: reconcile shared inbox keys and asset reuse buttons across zimage/storyboard/imagevideo/assets-summary.
- `ROLE_PA_TEMPLATE_BUILDER`: review Prompt Assistant custom template flow and overlap with text-to-text PA package.
- `ROLE_GENERATION_MODE_ROUTER`: check API/local/fallback mode routing and workflow prepare-only constraints.

Defer:

- `ROLE_PA_UX_LAUNCHER`: visual launcher polish can wait until contracts are stable.

## Safety Gates

- Do not modify `D:\DL\115\ComfyUI-aki-v1.3\ComfyUI-aki-v1.3`.
- Do not modify stable 7860.
- Do not expose or request `.env`, tokens, API keys, cookies, or browser profile content.
- Do not enable real generation unless owner gives action-time approval.
- Do not apply Web AI code blindly; use harvested files as contracts and patch plans.
