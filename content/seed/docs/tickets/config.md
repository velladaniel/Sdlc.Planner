# Ticket Tracker Configuration

Project-specific settings for the **ticket-sync** agent. This file is seeded once by the Sdlc.Planner package and never overwritten — edit it for your project. The workflow lives in `.github/agents/ticket-sync.agent.md`; the tool-level details for each tracker live in `.github/trackers/<provider>.md`. If a required value is still a `<PLACEHOLDER>`, the agent asks you for it and fills it in.

## Provider

Provider: `<jira | kaneo>`

Set it to `jira` or `kaneo`, then fill in only the matching section below and delete the other. To switch trackers later, change this value, fill in the other section, and re-run ticket-sync (existing `Ticket:` keys belong to the old tracker — clear them first or the agent will report them as unresolvable).

## Common

- **Story → ticket**: one ticket per story file; each `Tasks:` checklist line becomes a sub-task under it.
- **Group per domain**: every domain folder under `docs/userstories/<domain>/` maps to one group (Jira Epic / Kaneo group), created the first time a story from that domain syncs. The agent asks you for a descriptive group title before creating it.

## Jira

- **Jira project key**: `<JIRA_PROJECT_KEY>`
- **Jira URL**: `https://<your-tenant>.atlassian.net` (also set it in `.vscode/mcp.json`)
- **Team field** *(optional — delete these three lines if your Jira has none)*:
  - Field: `<TEAM_FIELD_ID>` (`cf[<TEAM_FIELD_NUMBER>]`)
  - Team IDs to scope every search to: `<TEAM_ID>[, <OTHER_TEAM_IDS>]`
  - Board filter it must match: `<BOARD_FILTER_URL>`

## Kaneo

- **Kaneo URL**: `https://<your-kaneo-host>` (also set it in `.vscode/mcp.json`)
- **Workspace**: `<KANEO_WORKSPACE>`
- **Project**: `<KANEO_PROJECT>`
- **Group mode**: `parent-task` *(or `label`)*
- **Column mapping** *(the agent fills this in from `list_project_columns` on first use)*:
  - `new`: `<slug>`
  - `in-progress`: `<slug>[, <slug>]`
  - `done`: `<slug>`
  - **New column** (where created stories go): `<slug>`
  - **Done column**: `<slug>`
  - **Cancelled column** *(optional)*: `<slug>`
