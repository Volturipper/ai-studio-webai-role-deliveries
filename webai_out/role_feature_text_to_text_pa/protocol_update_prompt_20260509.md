ROLE_ID: ROLE_FEATURE_TEXT_TO_TEXT_PA

Protocol update. Continue your assigned AI Studio Prompt Assistant work, but optimize for low-token collaboration.

Rules:
- Do not paste long reports into chat.
- You may save/package your own role-owned work every iteration if that helps preserve progress.
- Keep files under webai_out/role_feature_text_to_text_pa/ and use ASCII filenames.
- Codex will not harvest every round. Codex will harvest only when you explicitly mark HARVEST_READY or DONE, or when a monitorable marker says the output is worth collecting.
- If you are still iterating, reply only with:

STATUS: ITERATING | BLOCKED | NEEDS_OWNER_DECISION
ROLE_ID:
CHANGED:
BLOCKER:
NEXT:

- If your assigned work is mature enough to collect, reply only with:

STATUS: HARVEST_READY | DONE | FILES_READY
ROLE_ID:
FILES:
SUMMARY:
DELIVERY_ROUTE:

Maturity target:
- A coherent patch plan or final files for Prompt Assistant user-facing behavior.
- Clear selectors/APIs touched.
- Smoke-test checklist Codex can run.
- No secrets, no API keys, no .env, no cookies, no browser profile content.

If you need independent review, say NEED_REVIEW and identify which role should review it. Otherwise continue until HARVEST_READY or DONE.
