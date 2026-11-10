# Risk Log

A living log of project risks — things that *might* go wrong and need ongoing tracking, monitoring, or mitigation — kept separate from one-time assumptions and decisions.

**One file per risk**, so two people (or two feature branches) logging risks at the same time never edit the same file or line and never hit a merge conflict.

- **Not the same as `pid.md`'s `Risks` section**: that's a one-time kickoff snapshot. This folder tracks risks as they evolve.
- **Not the same as `Open Questions / Assumptions`** in `reqs/<domain>.md` or `Assumptions & Open Questions` in `architecture.md`: those are one-time guesses to confirm. Promote one to a risk only if it's ongoing and needs monitoring or mitigation.
- **Not the same as [technical-decisions/](../technical-decisions/)**: that logs decisions already made.

## How This Log Works

- **File name = risk ID**: `YYYY-MM-DD-short-kebab-slug.md` (for example `2026-11-09-shared-ui-package-coupling.md`), using the date the risk was raised. There are no sequential numbers and no index file — both are merge-conflict magnets. If two risks share a date and slug, add a suffix to the slug. Cite a risk by its file name without the extension.
- **Finding risks**: list the folder (names sort chronologically) or search for `Status: Open`. Don't maintain a hand-written summary table.
- **Categories**: `Business`, `Technical`, `Delivery`, `Compliance`, `Other`.
- **Severity**: `Likelihood` and `Impact` are each `Low` / `Medium` / `High`.
- **Status lifecycle**: `Open` → `Mitigated` → `Closed`, with side-states `Accepted` (tolerable as-is) and `Materialized` (it happened).
- **Ownership**: **business-analyst** owns Business/Compliance risks, **architect** owns Technical risks, **product-owner** owns Delivery risks, **automation-qa** owns automation risks. The owner logs new ones, keeps existing ones current (status, mitigation) whenever its work touches them, and tidies its own category. `Other` risks are maintained by you. You can add, update, or close any entry directly.
- **Keep the log clean — git holds the history**: merge duplicate or overlapping risks into one entry, and delete entries that are `Closed` and no longer informative (or that were superseded by a broader risk). **Always confirm deletions with the user first** (list the files and why); never delete an `Open` or `Materialized` entry.
- **Immutability**: don't rewrite an entry's `Description` — update `Status` and append to `Mitigation/Contingency` so the history is preserved.

Start from [_template.md](_template.md).
