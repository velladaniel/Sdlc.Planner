# Tracker adapter: Jira

Used by the **ticket-sync** agent when `docs/tickets/config.md` has `Provider: jira`. Values in `<angle brackets>` come from the `## Jira` section of that config. Managed by the Sdlc.Planner package — don't edit it in a consuming repo; change project-specific values in the config instead.

## Setup

Uses the `mcp-atlassian` MCP server (container). Requires Podman (or Docker — swap `"command"`) running. Add this to `.vscode/mcp.json` (merge into any existing `servers`/`inputs`), setting `JIRA_URL`:

```json
{
  "inputs": [
    { "id": "jira-username", "type": "promptString", "description": "Your Atlassian/Jira account email" },
    { "id": "jira-api-token", "type": "promptString", "description": "Jira Cloud API token (create at https://id.atlassian.com/manage-profile/security/api-tokens)", "password": true }
  ],
  "servers": {
    "mcp-atlassian": {
      "type": "stdio",
      "command": "podman",
      "args": [
        "run", "-i", "--rm",
        "-v", "${workspaceFolder}/docs/designs:/app/data/designs:ro",
        "-e", "JIRA_URL", "-e", "JIRA_USERNAME", "-e", "JIRA_API_TOKEN", "-e", "JIRA_SSL_VERIFY",
        "ghcr.io/sooperset/mcp-atlassian:latest"
      ],
      "env": {
        "JIRA_URL": "https://<your-tenant>.atlassian.net",
        "JIRA_USERNAME": "${input:jira-username}",
        "JIRA_API_TOKEN": "${input:jira-api-token}",
        "JIRA_SSL_VERIFY": "true"
      }
    }
  }
}
```

## Scoping rule

If the config defines a Team field, **every** JQL search must include `AND "cf[<TEAM_FIELD_NUMBER>]" IN (<TEAM_IDS>)` (matching the team's board filter), never just `project = <JIRA_PROJECT_KEY>`. Without a Team field, scope by project only.

## Operations

| Operation | Implementation |
|---|---|
| **FindGroup** | `jira_search`: `project = <JIRA_PROJECT_KEY> AND issuetype = Epic AND summary ~ "<domain>"` (+ scoping rule) |
| **CreateGroup** | `jira_create_issue`, type `Epic`, summary = group title, Team field set if configured |
| **FindStory** | `jira_search`: `project = <JIRA_PROJECT_KEY> AND issuetype = Story AND "Epic Link" = <GROUP_KEY> AND summary ~ "<Title>"` (+ scoping rule) |
| **CreateStory** | `jira_create_issue`, type `Story`, project, Team field if configured; then `jira_link_to_epic` to the group |
| **UpdateStory** | `jira_update_issue` |
| **GetStory** | `jira_get_issue` (includes sub-tasks, attachments, status) |
| **CreateSubtask** | `jira_create_issue`, type `Sub-task`, parent = the Story |
| **FindSubtask** | `jira_get_issue` on the Story and match sub-task summaries to the task text |
| **CompleteSubtask** | `jira_transition_issue` to the workflow's Done status |
| **ResolveStatusCategory** | `jira_get_issue` → status category: `new` → new, `indeterminate` → in-progress, `done` → done |
| **Cancel** | `jira_transition_issue` to the workflow's Cancelled / Won't Do status (list transitions first; skip if already terminal) |
| **AttachFile** | `jira_update_issue` with the `attachments` parameter. The server runs in a container, so pass **container paths**: `docs/designs/` is mounted read-only at `/app/data/designs/`, so `docs/designs/_shared/app-shell.png` → `/app/data/designs/_shared/app-shell.png`. Check existing attachments via `jira_get_issue` first. |
| **RemoveAttachment** | Not available through the MCP tools — leave the flag and ask the user to remove it manually. |

## Notes

- Issue types and the Team custom field vary per Jira site; if a create fails on an unknown type or field, ask the user and record the corrected value in the config.
- Estimates are never mapped to Jira fields; they stay as descriptive text in the description.
