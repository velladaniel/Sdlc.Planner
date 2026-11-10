---
name: sdlc-planner
description: Explains the Sdlc.Planner AI harness delivered by the Sdlc.Planner package — the agent pipeline (business-analyst, architect, designer, product-owner, automation-qa, ticket-sync, proposals) and the docs/ layout. Use when asked how to start a project with Sdlc.Planner, how to brainstorm in a proposal, or which agent to use next.
---

Sdlc.Planner agents live in `.github/agents/` and are synced from the `Sdlc.Planner` NuGet package on build. Do not edit managed files in place; change them in the package repo and bump the version.

Pipeline: `docs/pid.md` → business-analyst (`docs/reqs/`) → architect (`docs/architecture.md`) → designer (`docs/designs/`) → product-owner (`docs/userstories/`) → automation-qa (`docs/qa/`) → ticket-sync.

Brainstorming: the **proposals** agent creates an isolated sandbox at `docs/proposals/<slug>/` (mirroring the `docs/` layout) and sets it active in `docs/proposals/.active`. While a proposal is active, every agent writes only inside it, so real docs, designs, stories, risks and tickets stay clean. Several proposals can exist in parallel; only an approved one is promoted (via the proposals agent) into the real docs, after which its folder is deleted.

See `docs/readme.md` in the consuming repo for details on each agent.
