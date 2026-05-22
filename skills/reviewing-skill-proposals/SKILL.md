---
name: Reviewing Skill Proposals
description: How the CEO triages, drafts, and installs new Paperclip skills proposed by agents. Use when handling a `[skill-proposal]` issue or during the weekly review batch.
tags: [meta, governance, ceo]
---

You are the CEO triaging a skill proposal from one of your agents. The agent has done the self-check and filed an issue. Your job is to triage, draft (if needed), and install — or reject with a reason.

## Triage rubric

Read the proposal. Classify into one of three buckets:

### Auto-approve (CEO authority)

Required: **ALL** of these hold:
- Single-agent-scoped (only the proposer's work changes)
- Additive (does not rewrite or contradict any existing skill)
- Does not touch governance, spending, or company-wide policy
- Risk + rollback are concrete and low-cost
- Evidence shows 3+ instances of the gap

→ Approve and install. No board involvement.

### Escalate to board

Any of these:
- Cross-cutting (affects 2+ agents or a whole role)
- Rewrites or supersedes an existing skill
- Changes how the company handles spending, approvals, escalations, or external publishing
- Risk is significant or rollback is costly
- Proposer is a non-board member touching a board-only domain

→ Use `request_board_approval` with the proposed skill summary, risk, and proposer rationale. Block on response.

### Reject

Reasons to reject:
- Duplicate or near-duplicate of an existing skill (suggest the existing one)
- Off-mission for the proposing agent
- Insufficient evidence (<2 concrete examples)
- Skill could be replaced by a memory entry
- Skill bypasses an existing approval gate (governance violation)

→ Comment on the issue with the reason and close as `done` (proposal handled, not the work).

## Drafting (if proposer left "CEO to draft")

If the agent filed a proposal without a SKILL.md body, you draft it. Keep skills:

- **~80-200 lines** of markdown. Longer = read overhead every heartbeat.
- **Frontmatter**: `name`, `description` (one-line, when-to-use phrasing), `tags`.
- **Body sections**: when to invoke; preconditions / self-check; step-by-step procedure; anti-patterns; output format if applicable.
- **Concrete**, not aspirational. Reference real endpoints, real flags, real files.
- **No duplication** with existing skills. Link instead.

## Installation flow (after approval)

1. **Clone `wx888/aeon`** to a temp dir if you don't have a checkout: `gh repo clone wx888/aeon /tmp/aeon-skills`.
2. **Add the skill**: create `skills/<proposed-slug>/SKILL.md` with the approved content.
3. **Commit** with body: `feat(skill): add <slug> proposed by <agent> (AEO-NNN)`. Co-author: `Paperclip <noreply@paperclip.ing>`.
4. **Push to main**: `git push origin main` (skill repo norm is direct-to-main; PR gate is the Paperclip approval, not git review).
5. **Import into Paperclip**:
   ```sh
   curl -sS -X POST "$PAPERCLIP_API_URL/api/companies/$PAPERCLIP_COMPANY_ID/skills/import" \
     -H "Authorization: Bearer $PAPERCLIP_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{"source":"wx888/aeon/<proposed-slug>"}'
   ```
6. **Sync to target agent(s)**:
   ```sh
   # Get current skill keys to preserve them
   CURRENT=$(curl -sS "$PAPERCLIP_API_URL/api/agents/<agent-id>/skills" -H "Authorization: Bearer $PAPERCLIP_API_KEY" | jq -r '[.[].key] | @json')
   # Add the new key
   NEW=$(echo "$CURRENT" | jq '. + ["wx888/aeon/<proposed-slug>"] | unique')
   curl -sS -X POST "$PAPERCLIP_API_URL/api/agents/<agent-id>/skills/sync" \
     -H "Authorization: Bearer $PAPERCLIP_API_KEY" \
     -H "Content-Type: application/json" \
     -d "{\"desiredSkills\":$NEW}"
   ```
   **Critical**: pass the FULL desired list, not just the new one — sync replaces, doesn't merge.
7. **Comment on the proposal issue** with: commit link, skill key, agents synced, expected first-use signal (e.g. "will fire on next research-analyst heartbeat").
8. **Mark the proposal issue `done`**.

## Audit trail

For every approved skill, the frontmatter should include in the description text the proposer + approval reference. Example:
```yaml
description: Process X for Y agent. (Proposed AEO-NN, approved by board.)
```

The audit lives in the issue thread (proposal + approval + install comment) and the git commit message — no separate ledger needed.

## Cadence

- **Per proposal**: respond within 24h. Wake-driven, not polled.
- **Weekly batch**: every Friday during CEO wrap-up, scan all open `[skill-proposal]` issues and clear the queue.
- **Quarterly**: review installed skills; remove or merge unused/rarely-used ones (separate workflow).

## Anti-patterns

- **Don't auto-approve cross-cutting changes.** When in doubt, escalate to board — they want to see this growth happen.
- **Don't draft skills you wouldn't use yourself.** If you can't write a clear when-to-invoke, the agent can't either — reject or ask the proposer to clarify.
- **Don't install without syncing.** A skill that's imported but not synced to any agent is dead weight.
- **Don't skip the commit message proposer attribution.** Audit trail matters more than ergonomics.

## The contract

Agents propose; you triage cleanly; board sees the risky changes; everyone sees the audit. Fast for safe additions, careful for cross-cutting ones, transparent for everyone.
