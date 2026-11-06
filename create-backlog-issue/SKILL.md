---
name: create-backlog-issue
description: Create a GitHub issue in picklecillo/cursos and place it in the "Plataforma cursos" project board with Status=Backlog. Use when user asks to create a backlog issue, file something on the backlog, or add an issue to the Backlog column.
---

# Create Backlog Issue

Creates a GH issue and puts it on the project board's **Backlog** status column. "Backlog" here is a project board *Status* value, not a GH label — this repo has no `Backlog` label, don't create/apply one.

## Project IDs (picklecillo/cursos, project "Plataforma cursos")

- Project number: `1`, owner: `picklecillo`
- Project ID: `PVT_kwHOADyEjM4Bd9Oe`
- Status field ID: `PVTSSF_lAHOADyEjM4Bd9OezhYa9GQ`
- Backlog option ID: `6d69bafe`

If any `gh project` command below fails (e.g. project restructured), re-fetch fresh IDs:
```
gh project field-list 1 --owner picklecillo --format json
```
and use the current `Status` field id + `Backlog` option id instead of the hardcoded ones.

## Steps

1. Get a title and short body for the issue. If the user's request is ambiguous or underspecified, ask a clarifying question first (don't guess scope).
2. Confirm with the user before creating — issue creation is visible to others. Show the drafted title/body and ask to proceed (e.g. via AskUserQuestion).
3. Create the issue:
   ```
   gh issue create --repo picklecillo/cursos --title "<title>" --body "<body>"
   ```
4. Add it to the project board:
   ```
   gh project item-add 1 --owner picklecillo --url <issue-url>
   ```
   (This prints no output on success.)
5. Find the new item's project-item id — `item-add` doesn't return it, so look it up. **Must pass `--limit 200`**; the default limit is small and can omit the just-added item:
   ```
   gh project item-list 1 --owner picklecillo --format json --limit 200
   ```
   Filter items where `content.number` equals the new issue's number, take that item's `id`.
6. Set Status to Backlog:
   ```
   gh project item-edit --project-id PVT_kwHOADyEjM4Bd9Oe --id <item-id> \
     --field-id PVTSSF_lAHOADyEjM4Bd9OezhYa9GQ --single-select-option-id 6d69bafe
   ```
7. Report the final issue URL to the user.
