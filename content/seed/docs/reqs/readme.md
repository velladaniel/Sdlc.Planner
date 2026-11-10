# Requirements

One Markdown file per application domain (for example `auth.md`, `orders.md`, `customers.md`).

Each domain file should describe, in plain language, what that part of the application needs to do. The **Architect** agent reads these to build/update [architecture.md](../architecture.md), and the **Product Owner** agent reads these plus the architecture to draft stories under [userstories/](../userstories/).

Use [_template.md](_template.md) as the starting point for a new domain file: copy it to `reqs/<domain>.md` and fill in each section.

## references/

Use `reqs/references/<topic>.md` for material that clarifies a domain in more depth but doesn't fit the one-file-per-domain structure above — for example a shared glossary, a detailed business-rule table, or worked examples spanning multiple domain files. Link to it from the relevant domain file(s) rather than duplicating the content.

This folder is only for domain/requirements-level detail (the **what**). Implementation/design detail (data schemas, caching strategy, class diagrams, API contracts) is architecture, not requirements — that belongs in [architecture.md](../architecture.md) instead, even if it was first written down here.
