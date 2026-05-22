---
name: Proposing a Skill
description: How to propose a new Paperclip skill for yourself or another agent — file a structured issue, don't self-install. Use when you notice a recurring gap that a new skill would close.
tags: [meta, governance]
---

You are an agent who has noticed a gap that a new skill might close. **You cannot install new skills on yourself** — that requires CEO/board approval and a CEO-side install. What you CAN do is file a high-quality skill-proposal issue that makes the decision easy.

## When to propose

File a proposal when **any** of these hold:

1. **Recurring failure pattern** — the same kind of mistake or stuck heartbeat has happened 3+ times.
2. **Repeated coaching** — the board or your manager has corrected you on the same point twice or more.
3. **Manual work that should be playbook'd** — you've executed the same multi-step procedure from scratch 3+ times.
4. **Cross-agent value** — you discovered a pattern that would help other agents in the company too.

**Do not propose** for one-off needs, for skills that already exist (check first!), or to fix a single heartbeat you got wrong (use memory for that).

## Before you propose: self-check

1. **Search existing skills.** `GET /api/companies/$PAPERCLIP_COMPANY_ID/skills` — read every skill name and description. Is there one that already covers this?
2. **Search existing issues** for prior `[skill-proposal]` titles. Was this proposed and rejected? If so, what changed?
3. **Check your memory.** Could a memory entry close the gap instead of a new skill? Memory is cheaper than skills.

If any check passes, **don't file** — use the existing artifact.

## Cadence rule

Max **1 proposal per agent per week** unless emergency (e.g. you can't complete an assigned task without it). Quality over quantity.

## How to propose

Create an issue assigned to the CEO with title `[skill-proposal] <skill-name>: <one-line value>`.

Use this body template:

```markdown
## Problem

What gap or failure does this close? Be specific — name the failure mode, not the symptom.

## Evidence

- Link 1: past heartbeat / comment / failure where this would have helped
- Link 2: ...
- Link 3 (minimum 2, ideally 3+)

## Proposed skill

- **Name**: `proposed-skill-slug` (kebab-case)
- **Description**: one-line, ~120 chars, what it does and when to use
- **Who needs it**: which agent(s) — single-agent, role-scoped, or all agents
- **Tags**: e.g. `[research]`, `[ops]`, `[meta]`

## Draft skill body (optional but recommended)

Drop a first-pass SKILL.md body here, or leave blank and say "CEO to draft". Drafts speed approval.

## Expected impact

Concrete behavior change. Examples:
- "Cuts research-analyst heartbeat token cost by ~30% on source-digest runs"
- "Eliminates the need to re-read CLAUDE.md every routine fire"
- "Lets briefings-editor handle X without manager escalation"

## Risk + rollback

- What could go wrong if this skill is misapplied?
- How would we remove it cleanly if it produces bad output?

## Auto-approve eligibility

Mark this proposal **auto-approve eligible** if ALL hold:
- Single-agent-scoped (only your own work changes)
- Additive (does not rewrite or contradict any existing skill)
- Does not touch governance, spending, or company-wide policy
- Risk + rollback are concrete and low-cost

Otherwise it goes to board review.
```

## After filing

1. Set issue priority `medium` (or `high` if it's blocking your current work).
2. Continue your other work — the CEO wakes on the issue.
3. If approved + installed, you'll be woken with the new skill assigned and a follow-up comment naming the install.
4. If rejected, read the reason and adjust your approach. Don't re-file the same proposal.

## Anti-patterns

- **Don't propose a skill instead of using memory.** Memory handles single-agent, learned preferences. Skills handle repeatable procedures or playbooks.
- **Don't propose a skill that just re-wraps another skill.** If `digest` already does 80% of what you need, propose a `var:` value for it instead.
- **Don't propose a skill the day you joined.** Wait until you've hit the failure pattern 2-3 times.
- **Don't propose to bypass an existing approval.** A skill is not a way around board review for risky behavior.

## The contract

You propose; the CEO triages; the board approves cross-cutting changes; the CEO installs. You stay in your lane. In return, growth is audited, reversible, and visible to everyone.
