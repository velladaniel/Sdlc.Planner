# Proposals

Isolated workspaces for ideas you're still brainstorming. Nothing here is "real": the actual documentation, designs, stories, risks, decisions and tickets only contain approved work.

- Each idea lives in `docs/proposals/<slug>/` and mirrors the `docs/` layout (`reqs/`, `designs/`, `userstories/`, ...), plus a `proposal.md` cover sheet. See [_template.md](_template.md).
- Run several proposals at once; they're fully independent. List this folder to see them (no index file, to avoid merge conflicts).
- Talk to the **proposals** agent to create, list, summarise, switch, share, discard, or **promote** a proposal. Creating or switching sets `docs/proposals/.active` (git-ignored, local to you), so every other agent (business-analyst, designer, architect, ...) automatically works *inside* that sandbox and prefixes replies with `Proposal: <slug>`. Name a different proposal, or open one of its files, to override; say "leave the proposal" to go back to the real docs.
- Share with stakeholders via the generated `share.md` one-pager plus the proposal's `designs/` (or a branch containing only the proposal folder).
- Promotion happens only on your explicit approval, through a confirmed plan; afterwards the proposal folder is deleted. Rejected proposals are deleted too — git keeps the history.
- ticket-sync ignores `docs/proposals/**` entirely.

Full rules: [.github/instructions/proposals.instructions.md](../../.github/instructions/proposals.instructions.md).
