---
name: commit-msg-generator
description: >
  Generate professional Git commit messages by automatically running git diff
  and analysing the output. Use this skill whenever the user says "write a
  commit message", "generate commit msg", "what should I commit", "commit my
  changes", or asks Claude Code to stage or commit changes — even if they
  don't say "commit message" explicitly. Always use this skill before
  composing any git commit message, no exceptions. Do NOT wait for the user
  to paste a diff — run the git command yourself first.
---

# Git Commit Message Generator

Automatically capture the current git diff and generate a clean, professional commit message from it.

---

## Step 1 — Capture the Diff

Run the following command in the repo root to get all staged and unstaged changes:

```bash
git diff HEAD
```

If the output is empty (nothing changed against HEAD), fall back to staged-only:

```bash
git diff --cached
```

If both are empty, tell the user:

> No changes detected. Make sure your files are modified or staged before generating a commit message.

And stop.

---

## Step 2 — Analyze and Generate

Analyze the diff and produce a commit message following the format and rules below.

---

## Output Format

```
<summary line: lowercase, imperative, no period>

* <Modification 1>
* <Modification 2>
...
```

- Leave **one blank line** between the summary and the bullet points.
- Do **not** use sentence case — all lowercase.
- Do **not** end lines with a period.

### Example

```
introduce Reporter class with basic report generation

* rename Reporte to Reporter
* implement generateReport to output total user count
```

---

## Rules

### Summary line
- Imperative mood, present tense: `add`, `fix`, `refactor`, `remove`, `introduce`, `update`
- Describe **what** the change accomplishes, not **how**
- Max ~72 characters
- All lowercase, no trailing period

### Bullet points
- Each bullet covers one **meaningful** change: functional, structural, or behavioral
- Group small related edits into a single bullet — do not list every line changed
- If the diff is mostly cleanup or formatting, say so clearly: `clean up whitespace and formatting in auth module`
- Do not invent or infer changes not visible in the diff
- Omit trivial implementation details (e.g., variable renames that don't affect behavior, import reordering)

### What to focus on
| Prioritise | Avoid |
|---|---|
| New functions / classes added | Listing every renamed variable |
| Behaviour changes | Describing obvious re-indentation |
| Structural refactors | Repeating the summary in bullets |
| Bug fixes | Adding context not in the diff |
| API / interface changes | |

---

## Edge Cases

| Situation | Behaviour |
|---|---|
| Diff is only whitespace / formatting | Single bullet: `clean up formatting` |
| Single-line change | Summary only, no bullets needed |
| Multiple unrelated changes in one diff | Group logically; note if the diff mixes concerns |
| Generated/minified file changed | Note it concisely: `update bundled assets` |
| Empty diff from both commands | Inform the user no changes were detected |
| Unreadable / binary-only diff | Note it concisely: `update binary assets` |

---

## Anti-Patterns to Avoid

- ❌ `Fixed stuff` — too vague
- ❌ `Changed line 42 in user.js` — too granular
- ❌ `This commit adds a new feature that allows users to…` — not imperative, too wordy
- ❌ Inventing changes not shown in the diff
- ❌ Using sentence case or capitalising the first word

---

## Step 3 — Verify Before Committing

After generating the commit message, always present it to the user and ask for confirmation before running any `git commit` command:

> Here's the generated commit message:
>
> ```
> <commit message>
> ```
>
> Does this look good? I'll commit once you confirm, or let me know what to change.

Do **not** run `git commit` until the user explicitly approves.

---

## Trailers

Do **not** append any git trailers, co-author lines, or signatures to the commit message.
This includes — but is not limited to:

```
Co-Authored-By: ...
Signed-off-by: ...
```

The output must end after the last bullet point. Nothing else.