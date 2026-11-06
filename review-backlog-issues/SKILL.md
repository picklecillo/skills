---
name: review-backlog-issues
description: Review every GitHub issue on the "Plataforma cursos" project board with Status=Backlog (picklecillo/cursos) and write or improve their descriptions. Use when user asks to review the backlog, clean up backlog issues, or write/fill in descriptions for backlog items.
---

# Review Backlog Issues

Walks every Backlog-status issue on the project board, checks if its description is thin/missing/stale, and writes an improved one grounded in the actual codebase — asking the user before guessing on ambiguous scope, and before writing any change.

## Project IDs (picklecillo/cursos, project "Plataforma cursos")

- Project number: `1`, owner: `picklecillo`
- If `gh project` commands below fail (e.g. project restructured), re-fetch fresh field/option ids:
  ```
  gh project field-list 1 --owner picklecillo --format json
  ```

## Steps

1. List all project items and filter to `Status == "Backlog"`:
   ```
   gh project item-list 1 --owner picklecillo --format json --limit 200
   ```
   Keep each item's `content.number`, `content.title`, `content.url` where the item is an Issue (skip PRs/drafts).

2. For each backlog issue, fetch its current body:
   ```
   gh issue view <number> --repo picklecillo/cursos --json number,title,body,url,labels
   ```

3. Judge the existing body. Flag as needing work if it's empty, is just a restatement of the title, lacks acceptance criteria / motivation, or looks stale vs. current code (e.g. references removed files/routes). Skip issues that already have a solid description — don't touch them, note them as "left as-is" in the final report.

4. For each issue that needs work, ground the new description in reality before writing it:
   - Check `docs/TASKS.md` for a matching task section (issues are 1:1 with TASKS.md sections per this repo's convention) and pull context from there.
   - Grep the codebase for related code (views, models, templates) to confirm current behavior/scope.
   - If motivation, scope, or acceptance criteria stay ambiguous after that research, **ask the user a clarifying question** (AskUserQuestion) rather than guessing. Batch related questions across issues where reasonable instead of interrupting once per issue.

5. Draft the improved body: what/why, current behavior if relevant, and a short acceptance-criteria checklist. Match the terse, factual style of existing well-written issues in the repo (skim 2-3 via `gh issue view` for tone before drafting the first one).

6. Confirm before writing — issue edits are visible to others. Present each draft (or batch of drafts) to the user and get a go-ahead, e.g. via AskUserQuestion or a plain summary + explicit "proceed?" ask.

7. Apply approved edits:
   ```
   gh issue edit <number> --repo picklecillo/cursos --body "<new body>"
   ```

8. Report a final summary: issues rewritten (with links), issues left as-is, and any issues skipped pending an unanswered clarifying question.
