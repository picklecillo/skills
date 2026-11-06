---
name: tidy-first
description: Apply small, safe, behavior-preserving structural cleanups (Kent Beck's "Tidy First?" tidyings) to the specific files about to change, committed separately from the behavior change. Use right before starting feature/bugfix work on existing code, or on explicit request ("/tidy", "tidy this", "tidy first").
---

# Tidy First

Structure changes and behavior changes never share a commit. Tidy (if it
helps), commit, then do the behavior change on the tidied code.

## When to run

- **Proactive**: about to edit an existing file/module for a feature or bugfix. Scan only the file(s) you're about to touch — not the whole app.
- **On-demand**: user says "tidy this", "/tidy", "tidy first", or similar.

## Core rule

A tidying is tiny, reversible, and preserves behavior — tests pass unchanged
before and after. Only do tidyings that make the **upcoming** change easier,
safer, or clearer. No drive-by refactors, no tidying unrelated code, no
tidying "because it'd be nicer" if it doesn't serve the change at hand.

If nothing in-scope helps the upcoming change, say so in one line and go
straight to the behavior work (TDD: failing test first, per this repo's
CLAUDE.md).

## Workflow

1. Identify the file(s) about to change and the behavior change intended.
2. Scan those files for tidying opportunities from the tactic list in
   [TACTICS.md](TACTICS.md) — but only ones that reduce friction for the
   upcoming change.
3. Report candidates found: `tactic name — location — one-line why it helps`.
4. Apply the tidyings that clearly help. Leave the diff purely structural —
   no renamed behavior, no new logic.
5. Stop. Do not commit. Tell the user the tidy diff is ready for its own
   commit (message prefix `tidy: <what>`, no `closes #<issue>` — that belongs
   to the behavior commit). Let the user's normal commit flow run separately.
6. Only then proceed to the behavior change (test first).

## Repo conventions this respects

- Business logic stays in `services.py`; tidying must not migrate logic
  across layers — that's a design change, not a tidying.
- Models stay thin — don't "tidy" by moving logic into/out of models.
- Templates: any tidying that touches user-facing strings must keep them
  wrapped in `{% translate %}` / `{% blocktranslate %}`.
- Tidy commits are exempt from `closes #<issue>`; the behavior commit that
  follows still needs it when tied to a GH issue.
- Verify tidyings don't break tests before handing off:
  `docker compose exec web uv run pytest apps/<app>`.

See [TACTICS.md](TACTICS.md) for the full tactic list with heuristics.
