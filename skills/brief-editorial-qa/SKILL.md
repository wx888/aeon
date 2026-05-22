---
name: brief-editorial-qa
description: Pre-delivery editorial QA checklist for the Briefings Editor — brand, citation, tone, subject-line, and delivery checks before any external brief is sent. (Proposed AEO-36, auto-approved by CEO 2026-05-22.)
tags: [editorial, quality, briefs, briefings-editor]
---

# Brief Editorial QA

Run this checklist BEFORE sending any external brief — morning brief, weekly review, research brief, or ad-hoc report. Catch brand, citation, and tone issues before they leave the house, not after.

## When to invoke

- Before sending any email or external-facing document.
- Whenever a brief is one step from delivery (`mail_send`, `send-email`, or equivalent).
- During the "DoD verification" phase of any brief-shaped task.

Do NOT invoke for internal Paperclip comments, issue updates, or agent-to-agent handoffs — this is for external content only.

## Preconditions

- The brief content is finalized in a file or buffer you can re-read.
- You know which recipient(s) and sender identity will be used.
- `BRIEF_RECIPIENTS` env var has been resolved (or is intentionally omitted with reason).

## Checklist — run every item, do not skip

### Brand compliance

- [ ] Sender display name is **"Happy Bear Research"** — not "Aeon", not "Briefings Editor", not the agent ID.
- [ ] No internal Paperclip references in the body (no `AEO-NN`, no agent names, no `interaction:` IDs).
- [ ] No agent identifiers, run IDs, or heartbeat metadata visible to the reader.
- [ ] Footer / signature is brand-consistent (Happy Bear Research, not internal).

### Content quality

- [ ] Subject line accurately reflects the top 1-2 findings — no clickbait, no generic "Daily update".
- [ ] Every factual claim has a source — inline link, citation, or named primary source.
- [ ] No hallucinated data — if a number or quote is uncertain, mark `(unverified)` or remove it.
- [ ] Research output is synthesized, not raw-pasted from analyst handoffs.
- [ ] No duplicate items across sections (same headline appearing twice).

### Tone and audience

- [ ] Written for the external reader (currently `nat.tan87@gmail.com`), not for an agent or the board.
- [ ] No internal jargon: avoid `heartbeat`, `interaction`, `routine`, `cron`, `approval-id`, etc.
- [ ] Consistent tense and register throughout (don't drift between formal and casual).
- [ ] Length matches brief type — daily ≤ 1 screen, weekly review can be longer.

### Delivery config

- [ ] Recipient address matches `BRIEF_RECIPIENTS` env var (or the user-specified override).
- [ ] `from` / `reply-to` is Happy Bear Research branded.
- [ ] If subject is ambiguous (e.g. "Test", "Re: ", empty), pause and confirm before sending.
- [ ] Send is gated by a real trigger (routine fire, explicit request) — not a stale wakeup loop.

## On failure

If any item fails:
1. Fix it in the brief content directly.
2. Re-run the relevant section (brand / content / tone / delivery) — fixes can introduce new failures.
3. Do NOT send until all items pass.

If a failure indicates a SYSTEMIC issue (e.g. brand directive missing from agent prompt, recipient env var unset), file a follow-up issue or update the agent config — don't just patch the one brief.

## Output

After the checklist runs:

- If all pass → proceed to send, then log "editorial QA: pass" in the issue comment alongside the send receipt.
- If any fail → log "editorial QA: fail — [item]" in the issue, fix, and re-run.

This log is what makes the QA auditable. No log = no QA happened.

## Anti-patterns

- **Don't run this from memory.** Re-read the actual brief content each time — what you intended and what's in the buffer can drift.
- **Don't skip items because "this brief is small".** A one-line brief can still have the wrong sender name.
- **Don't treat the checklist as a vibe check.** Each item is binary — pass or fail.
- **Don't auto-fix and continue silently.** Log the fix; the audit trail matters.

## Why this exists

This skill was proposed after the brand directive ("Happy Bear Research") was discovered mid-flight on AEO-9 instead of being caught pre-send. Three documented brief executions (AEO-9, AEO-14, AEO-15) applied editorial standards ad hoc with no codified gate. This skill makes pre-send QA explicit, auditable, and consistent across brief types.
