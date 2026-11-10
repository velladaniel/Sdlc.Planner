# [Domain Name] Requirements

> Copy this file to `reqs/<domain>.md` (e.g. `reqs/customers.md`) and fill in each section. Delete any section that's genuinely not applicable, but don't skip a section just because it's harder to answer — that's usually a sign the Architect agent will need to ask you about it.

## Summary

One or two sentences describing what this domain is and why it exists in the project.

## Actors & Roles

Who/what interacts with this domain, and under what role(s)/permission(s)?

- **Role name** — what they can do here, any permission strings relevant to RBAC (e.g. `customers.view`, `customers.edit`).

## Functional Requirements

The concrete capabilities this domain must provide. Prefer a numbered list of discrete, testable requirements over prose.

1. Users with role X can do Y.
2. The system must do Z when [event/condition].
3. ...

## Data Model

Key entities this domain owns, their important fields, and relationships to entities in other domains (name the other domain if the entity is owned elsewhere).

- **Entity** — key fields, relationships, ownership notes.

## Workflows / User Flows

Describe any multi-step processes or sequences worth capturing (e.g. as a numbered flow or a Mermaid sequence diagram) — login, approval flows, data sync, etc.

## Integration Points

How this domain talks to other domains/services, and what it depends on or exposes.

- **Depends on**: [other domain] for [reason].
- **Consumed by**: [other domain] for [reason].
- **API/events exposed**: rough shape of endpoints or events other domains rely on (detailed endpoint design belongs in `architecture.md`, but flag anything the architecture must account for).

## Non-Functional Requirements

Anything about performance, security, compliance, availability, scalability, or auditing that constrains the design.

- ...

## Frontend / UI Considerations

*(Only relevant if this domain has a user-facing UI.)*

- Does this domain need its own page/module, or does it live inside an existing area of the UI?
- Required roles/permissions to see it in navigation.
- Any other UI-specific configuration or constraints worth flagging for the architect.

## Open Questions / Assumptions

Anything you're unsure about or deliberately deferring. The Business Analyst and Architect agents will either assume-and-flag (minor/local) or ask you directly (high-impact/hard to reverse) — listing your own open questions here up front speeds that up.

- ...
