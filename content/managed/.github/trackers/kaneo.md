# Tracker adapter: Kaneo

Used by the **ticket-sync** agent when `docs/tickets/config.md` has `Provider: kaneo`. Values in `<angle brackets>` come from the `## Kaneo` section of that config. Managed by the Sdlc.Planner package — don't edit it in a consuming repo; change project-specific values in the config instead.

Kaneo is an open-source project tracker (<https://kaneo.app>). It has no Epic or Sub-task issue types, no Team field and no attachment tool in its MCP server, so this adapter maps those concepts onto tasks, relations and labels and lists the fallbacks explicitly.

## Setup

Uses Kaneo's MCP server. Either option exposes the same tools. Add one to `.vscode/mcp.json` (merge into any existing `servers`).

**Built-in HTTP endpoint (recommended)** — every Kaneo instance serves `/api/mcp`; VS Code signs you in via OAuth, no tokens in the repo:

```json
{
  "servers": {
    "kaneo": { "type": "http", "url": "https://<your-kaneo-host>/api/mcp" }
  }
}
```

**Stdio package** — needs Node.js 24+; authenticates with a device-code flow on the first tool call:

```json
{
  "servers": {
    "kaneo": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@kaneo/mcp", "serve"],
      "env": { "KANEO_API_URL": "https://<your-kaneo-host>" }
    }
  }
}
```

The server key must be `kaneo` — the ticket-sync agent's tool list refers to it as `kaneo/*`.

## Concept mapping

| Sync concept | Kaneo |
|---|---|
| Project / board | Kaneo **project** `<KANEO_PROJECT>` in workspace `<KANEO_WORKSPACE>` |
| Key | Task ticket id such as `KAN-12` (use `get_task_by_ticket_id`; pass `projectId` if ambiguous) |
| Group (per domain) | Per `Group mode` in the config: **`parent-task`** (default) — a task titled with the group title and labelled `group`, whose stories are linked as its subtasks; or **`label`** — no group task, each story gets the label `group:<domain>` and the group "key" in `groups.md` is that label name |
| Story | A task in the project |
| Sub-task | A task linked as a subtask of its Story |
| Status category | Derived from the project's columns (below) |

## Operations

| Operation | Implementation |
|---|---|
| **FindGroup** | `parent-task`: `search` / `list_tasks` in the project for a task labelled `group` whose title matches the domain. `label`: `list_workspace_labels` for `group:<domain>`. |
| **CreateGroup** | `parent-task`: `create_task` (title = group title) + `attach_label_to_task` with label `group` (`create_label` first if missing). `label`: `create_label` `group:<domain>`. |
| **FindStory** | `list_tasks` / `search` in the project for a task with the matching title that is related to the group (or carries the group label) |
| **CreateStory** | `create_task` in `<KANEO_PROJECT>` with the description built by the agent; status = the config's `New column`; then link to the group (`create_task_relation` as a subtask of the group task, or `attach_label_to_task`) |
| **UpdateStory** | `update_task` |
| **GetStory** | `get_task` / `get_task_by_ticket_id` (+ `get_task_relations` for sub-tasks, `list_task_comments` if needed) |
| **CreateSubtask** | `create_task` + `create_task_relation` marking it a subtask of the Story. Read the relation tool's input schema and use its parent/subtask relation type — don't guess type names. |
| **FindSubtask** | `get_task_relations` on the Story and match titles |
| **CompleteSubtask** | `update_task_status` to the config's `Done column` (skip if already there) |
| **ResolveStatusCategory** | `get_task` → current column slug → category via the config's column mapping (below) |
| **Cancel** | `update_task_status` to the config's `Cancelled column` if set; otherwise move to the `Done column` **and** `attach_label_to_task` with label `cancelled` so it is not mistaken for completed work |
| **AttachFile** | **Unsupported** by the MCP server. Fallback: add a `Design` section to the description listing each image's repo-relative path (e.g. `docs/designs/_shared/app-shell.png`) so reviewers can open it from the repo; report that images were linked, not attached. |
| **RemoveAttachment** | Unsupported — if the image was only linked in the description, remove that line via `update_task`; otherwise flag it for manual removal. |

## Status mapping

Column slugs are project-specific. On first use call `list_project_columns` for `<KANEO_PROJECT>`, show the user the slugs, and write the result into the config's `Column mapping` (which slug is `new`, which are `in-progress`, which are `done`, and optionally a `Cancelled column`). Always map by that table, never by guessing from column names. The slugs `list_project_columns` returns are exactly the values `create_task` and `update_task_status` accept.

## Notes

- Kaneo has no Team field; the config's team settings are ignored.
- Task descriptions are Markdown, so the story content can be pasted as-is.
- If the Kaneo MCP server is updated and a tool named here no longer exists or has changed, ask the user and propose an update to this adapter rather than improvising.
