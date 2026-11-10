# Technical Decisions

A log of significant technical decisions made for this project — the "why" behind choices in [../architecture.md](../architecture.md), kept separately so the reasoning survives even as the architecture itself evolves.

**One file per decision**, so two people (or two feature branches) logging decisions at the same time never edit the same file or line and never hit a merge conflict.

## How This Log Works

- **File name = decision ID**: `YYYY-MM-DD-short-kebab-slug.md` (for example `2026-11-09-entra-id-identity-provider.md`), using the date the decision was made. There are no sequential numbers and no index file — both are merge-conflict magnets. If two decisions share a date and slug, add a suffix to the slug. Cite a decision by its file name without the extension.
- **Finding decisions**: list the folder (names sort chronologically) or search for `Status: Accepted`. Don't maintain a hand-written table of contents.
- **When to add an entry**: whenever a decision is "high-impact / hard to reverse" (a technology/pattern choice affecting multiple domains, a structural change, anything future work builds on). Small, easily-revised details stay as inline `Assumptions & Open Questions` notes in `architecture.md` or a domain's `reqs/<domain>.md`.
- **Statuses**: only two — `Accepted` or `Superseded by <decision-id>`. A decision is logged once it's actually been made.
- **Immutability**: never edit an existing entry's Context/Decision/Consequences. If a decision changes, add a *new* file with a `Supersedes:` line and change only the old file's `Status` line to `Superseded by <new-decision-id>`.
- **Keep the log clean — it describes current truth, git holds the history**: a superseded entry is only worth keeping while it explains a decision still in play. Prune stepping stones:
  - If a chain is reversed (A is superseded by B, B by C, and the project then returns to A's approach), **reinstate A** (set `Status: Accepted`, add a `Reinstated: YYYY-MM-DD — <why>` line, remove its `Superseded by` line) and **delete B and C** — they are no longer needed.
  - Delete any `Superseded` entry whose replacement fully captures the reasoning and which nothing else references. Keep one only if its "alternatives rejected" rationale is still useful to future readers.
  - After deleting, fix any remaining `Supersedes:` / `Superseded by` lines so none point to a missing file.
  - **Always confirm deletions with the user first** (list the files and why); never delete an `Accepted` entry.
- **Who conforms**: the **architect** and **business-analyst** agents read every file in this folder before making changes, and must not silently contradict an `Accepted` entry — a conflict is treated as high-impact and raised with you via a clarifying question.
- **You can add or edit entries directly** at any time; agents treat them as equally authoritative.

Start from [_template.md](_template.md).
