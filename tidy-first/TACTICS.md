# Tidying tactics

Each is behavior-preserving. Apply only where it serves the change you're
about to make.

1. **Guard Clauses** — nested `if`/`else` guarding the "real" logic → early
   `return`/`continue`/`raise` at top, flatten the rest.

2. **Dead Code** — unused function/branch/import/var → delete it. Never
   comment out; git history is the record.

3. **Normalize Symmetries** — two code paths that do similar things but are
   written differently (different order of checks, different naming for the
   same role) → make them structurally identical so the real differences
   stand out.

4. **New Interface, Old Implementation** — need a cleaner call shape but the
   old implementation still works → add the new interface calling straight
   into the old one, migrate callers one at a time, delete the old interface
   once unused.

5. **Reading Order** — reader hits a name before its definition, or has to
   jump around → reorder declarations/functions so the file reads top-to-
   bottom in the order a new reader needs them.

6. **Cohesion Order** — things that change together are declared far apart →
   move them adjacent.

7. **Move Declaration and Initialization Together** — a variable is declared
   in one place and assigned its real value several lines later → collapse
   to one spot.

8. **Explaining Variables** — a gnarly subexpression inline in a condition or
   call → pull into a well-named local variable.

9. **Explaining Constants** — a magic number/string literal → name it as a
   constant.

10. **Explicit Parameters** — a function pulls extra state from a shared
    object/closure instead of taking it as a parameter → make the dependency
    an explicit argument.

11. **Chunk Statements** — a long function body with implicit phases →
    separate each phase with a blank line (add a short comment only if the
    blank line alone doesn't make the phase boundary obvious).

12. **Extract Helper** — a coherent chunk inside a long function has a name-
    able purpose → pull it into a helper function named for that purpose.

13. **One Pile** — an over-decomposed set of tiny helpers obscures the shape
    of the real change → inline them back into one function first, *then*
    re-extract along the lines the new change actually needs.

14. **Explaining Comments** — code can't make some invariant or "why" self-
    evident → add a short comment stating it.

15. **Delete Redundant Comments** — a comment just restates what the code
    already says → delete the comment.
