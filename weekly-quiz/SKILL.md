---
name: weekly-quiz
description: Quizzes the user on concepts noted during the current or specified week (Mon-Sun) from their Obsidian vault. Includes gap analysis, retry rounds, and persistent state for missed concepts across sessions.
---

# Weekly Concept Recall Quiz (`weekly-quiz`)

Quizzes the user on concepts from their Obsidian daily notes and linked concept files. Tracks missed concepts and audits note gaps.

## Obsidian Vault Configuration
- **Daily Notes Directory:** `/Users/gautham/Dropbox/obsidian/vault/80 Daily Note/`
- **State Persistence File:** `~/.gemini/antigravity-cli/quiz_state.json`

---

## Tone & Style
- Be terse. No lengthy prose. Feedback = bullet points or a short code snippet only.
- **Vague answer rule:** If the user's answer is a vague phrase (e.g. "this means this", "it's like a pattern", "some hack") — reject it immediately. Say: _"That's too vague — restate it clearly and specifically."_ Do not evaluate or award credit until they do.

---

## Workflow Steps

### Step 1: Range Confirmation
1. Determine current week's dates (Monday–Sunday/today).
2. Scan the Daily Notes Directory for `YYYY-MM-DD.md` files in range.
3. Show the date range + files found. Ask one line: _"Proceed?"_

### Step 2: Note Parsing & Concept Extraction
1. Read daily notes for the confirmed range.
2. Follow any `[[wikilinks]]` referenced.
3. Categorize:
   - **Valid Concepts Pool** — clear concepts ready for testing.
   - **Note Gaps & Errors Pool** — missing context, factual errors, incomplete fragments. (Set aside for Step 6.)

### Step 3: Warm-up (Previous Missed Concepts)
1. Read the state file if it exists.
2. If there are unresolved missed concepts, ask those first.

### Step 4: Main Quiz Round (One-by-One)
1. Ask open-ended questions **one at a time**.
2. For each:
   - If vague → reject, ask to restate (see Tone & Style).
   - Evaluate: list key points hit vs missed (tersely).
   - Give the **Recommended Answer** (terse — bullets or snippet).
   - If incorrect or incomplete → add to Retry Queue.

### Step 5: End-of-Session Retry Round
1. Re-test Retry Queue concepts once.
2. Update state file:
   - Remove correctly answered concepts.
   - Persist remaining misses for next week's warm-up.

### Step 6: Post-Quiz Audit Report
1. Present a short "Note Gaps Report":
   - Incomplete fragments, ambiguous notes, or factual errors from Step 2.
   - One-line recommendation per gap.
