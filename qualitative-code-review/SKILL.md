---
name: qualitative-code-review
description: Terse, bullet-only qualitative review of code structure, naming, coupling, cohesion, modularity, and readability — no fixes suggested, no explanations, no sugarcoating. Invoke explicitly (e.g. "/qualitative-code-review") when the user wants a blunt architectural critique rather than a bug hunt or fix-it pass. Do not trigger on generic "review my code" or "review this diff" requests — those belong to the separate code-review plugin, which checks correctness and suggests fixes.
---

# Qualitative Code Review

This is a structural and stylistic critique, not a bug hunt. The point is to surface
design smells — the kind of thing a sharp reviewer would flag in a design doc review,
not a PR comment thread. Don't drift toward correctness bugs, security issues, or
performance — that's a different job (the `code-review` plugin handles it).

## What to look at

Judge the code against these dimensions:

- Separation of concerns — UI vs logic vs fetching vs control flow
- Modularization and abstraction — is anything doing too much, or split for no reason
- General code hygiene
- Global scope pollution — variable declarations in global scope (constant strings are fine)
- Consistency — style and pattern consistency within and across files
- Naming, and the order of function arguments
- Dead or redundant logic
- Readability
- Complexity — cyclomatic complexity, function fragmentation
- Module loading order / circular dependencies
- File and code organization
- Encapsulation of state — global state, mutable shared state

## How to review

Read the target code in full before writing anything — a partial read produces a
partial (and often wrong) verdict on cohesion and coupling, since those are properties
of the whole file or module, not any single function.

Then write the review as a flat bullet list. Each bullet names one specific issue at
one specific location (file/function/line) — not a category summary. "Naming is
inconsistent" is not a bullet; "`fetchData` and `get_user` in the same file mix
camelCase and snake_case" is.

## Output rules

The output is a signal for someone who already knows the codebase and wants the
verdict, not a teaching moment — so keep it stripped down:

- Bullet points only. No prose paragraphs, no headers, no summary at the top or bottom.
- State the problem. Don't propose the fix or the alternative.
- Don't explain why something is a problem — assume the reader already knows.
- Don't soften findings ("might want to consider...") — say what's wrong.
- If a dimension has no issues, skip it silently. Don't write "naming: no issues."
- Skip praise. This review only enumerates problems.
