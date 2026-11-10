---
applyTo: "docs/proposals/**"
description: 'Rules for proposals: isolated brainstorming/work-in-progress workspaces that never touch the real docs until promoted.'
---
# Proposals

A **proposal** is an isolated sandbox for an idea you're still exploring — requirements, architecture notes, designs, risks, decisions, even draft stories — that you may show to stakeholders, iterate on, run alongside other ideas, discard, or promote. **The real documentation (`docs/reqs/`, `docs/architecture.md`, `docs/designs/`, `docs/userstories/`, `docs/technical-decisions/`, `docs/risk-log/`, `docs/project-overview.md`, `docs/qa/`) and the ticket tracker only ever contain approved work.**

## Layout

```
docs/proposals/<slug>/
  proposal.md              # the cover sheet (from docs/proposals/_template.md)
  reqs/<domain>.md         # mirrors docs/ — same relative paths as the real thing
  architecture.md          # delta/notes for this idea, not a full copy
  designs/...              # mockups to show stakeholders
  userstories/<domain>/... 
  technical-decisions/YYYY-MM-DD-slug.md
  risk-log/YYYY-MM-DD-slug.md
  qa/<domain>.md
```

- `<slug>` is short kebab-case (e.g. `self-service-onboarding`). Several proposals can exist side by side; each is fully independent. There is **no index file** — list `docs/proposals/` to see them.
- Inside a proposal, mirror the real `docs/` paths and file formats exactly (use the same templates), so promotion is a straight move.
- `architecture.md` and `designs/design.md` in a proposal are **deltas**: they describe only what this idea adds/changes relative to the real file, and name the real sections affected. Never copy the whole real file in.
- **Mockups in a proposal** reuse the real design system: load the real `docs/designs/_shared/theme.js` / `shell.js` with the correct relative path (from `docs/proposals/<slug>/designs/<domain>/x.html` that is `../../../../designs/_shared/theme.js`). New or changed shared components go in the proposal's own `designs/_shared/`. The real `docs/designs/design-guide.html` is not touched until promotion.
- A proposal may reference real docs (read them freely) but never edits them.

## Statuses (in `proposal.md`)

`Exploring` → `Shared` (shown to stakeholders) → `Approved` (ready to promote) or `Rejected`. Promoted and rejected proposals are **deleted** — git history keeps them.

## Which proposal am I in?

Resolve the proposal slug in this order, and stop at the first hit:

1. A proposal named in the user's message.
2. The active editor file, if it is under `docs/proposals/<slug>/`.
3. The slug stored in `docs/proposals/.active` (a git-ignored, one-line file holding the current slug) — this is how the user "stays" on a proposal across chats without repeating it.
4. Otherwise, if the request sounds like brainstorming, ask via #tool:vscode/askQuestions (list existing slugs, offer to create one).

Safeguards against a stale pointer:
- Start **every** reply in proposal mode with `Proposal: <slug>`, so a wrong sandbox is obvious immediately.
- If sources 1 or 2 name a different proposal than `.active`, use the named one for this request and ask whether to switch `.active`.
- If `.active` names a folder that no longer exists, ignore it, work on the real docs, and tell the user to clear it with the `proposals` agent.
- The user can say "leave the proposal" / "work on the real docs" to clear it; with no active proposal, agents work on the real docs as normal.
- Only the `proposals` agent writes `.active` (on create, switch, leave, promote and discard).

## Proposal mode — rules for every agent

When the user says they are brainstorming / exploring / drafting a proposal, or a proposal is resolved by the order above:

1. Identify the proposal slug as described above.
2. **Write only inside `docs/proposals/<slug>/`**, mirroring the real path. Do not edit anything else — including `docs/project-overview.md`, the risk log, technical decisions, and `docs/tickets/` files.
3. **Do not sync to the ticket tracker** and do not treat proposal stories as `Ready`. The ticket-sync agent ignores `docs/proposals/**` completely.
4. Read the real docs as the baseline. Where the proposal contradicts an `Accepted` technical decision or existing requirement, record it under *Conflicts with current docs* in `proposal.md` instead of resolving it silently.
5. Keep `proposal.md` current: summary, status, what files exist, open questions, conflicts, and stakeholder feedback received.
6. Keep proposals tidy: delete stale drafts and scratch files inside the proposal as you iterate.

## Promotion

Only the `proposals` agent promotes, and only when the user explicitly approves. See its workflow. After promotion the proposal folder is deleted and the normal pipeline takes over (architect validation → designer → product-owner → automation-qa → ticket-sync).
