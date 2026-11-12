# Sdlc.Planner

[![NuGet](https://img.shields.io/nuget/v/Sdlc.Planner.svg)](https://www.nuget.org/packages/Sdlc.Planner) [![Downloads](https://img.shields.io/nuget/dt/Sdlc.Planner.svg)](https://www.nuget.org/packages/Sdlc.Planner)

Published on [nuget.org](https://www.nuget.org/packages/Sdlc.Planner): `dotnet add <project> package Sdlc.Planner`.

NuGet package that delivers the Sdlc.Planner AI harness (Copilot agents, instructions and docs templates) into any repo, and keeps it up to date when the package version changes.

## What gets installed

| Kind | Files | Behaviour |
|---|---|---|
| **Managed** | `.github/agents/{business-analyst,architect,designer,product-owner,automation-qa,proposals,ticket-sync}.agent.md`, `.github/trackers/{jira,kaneo}.md`, `.github/instructions/*.instructions.md` | Overwritten on build when the package changes. Don't edit in the consuming repo. |
| **Seed** | `.github/copilot-instructions.md`, `docs/tickets/config.md`, `docs/{readme,pid,project-overview,architecture}.md`, `docs/proposals/{readme,_template}.md`, `docs/{technical-decisions,risk-log}/{readme,_template}.md`, `docs/reqs/{readme,_template}.md`, `docs/designs/design.md` | Copied only if missing, so project-specific content is never overwritten. Decisions and risks are one date-named file per entry (no sequential numbers, no index) so feature branches never conflict. `docs/tickets/config.md` and design.md contain placeholders to fill in. |

The package also ships `skills/sdlc-planner/SKILL.md` so skill-discovery tools (e.g. `dotnet-package-skills`) can pick it up.

## Use it in a project

Add to any project in the repo (the sync target runs on that project's build):

```xml
<PackageReference Include="Sdlc.Planner" Version="1.*" PrivateAssets="all" />
```

Then `dotnet build`. Files are written to the repo root: the nearest `.git` folder/file above the project, else the solution folder, else the project folder. NuGet has no solution-level packages, so reference Sdlc.Planner from **one** project (any will do). If the files land in the wrong place, pin the target in a `Directory.Build.props` at your repo root:

```xml
<Project>
  <PropertyGroup>
    <SdlcPlannerRoot>$(MSBuildThisFileDirectory)</SdlcPlannerRoot>
  </PropertyGroup>
</Project>
```

Or pass `-p:SdlcPlannerRoot=<path>` per build. Disable syncing with `-p:SdlcPlannerSync=false`.
Commit the generated files — that keeps agents available to Copilot on the web and for teammates.

## Update agents everywhere

1. Edit the files under `content/managed/` (or `content/seed/` for new-project templates) in this repo.
2. Bump `<Version>` in `Sdlc.Planner.csproj`.
3. `dotnet pack -c Release` and push the `.nupkg` to your feed (e.g. Azure Artifacts).
4. In each consuming repo: bump the package version and build; commit the updated `.github/` files.

To refresh a seed file, delete it in the consuming repo and rebuild.

## Local test

```powershell
dotnet pack -c Release -o ./artifacts
# in a test repo: add a local source pointing at ./artifacts, then
dotnet add package Sdlc.Planner --version 1.0.0
dotnet build
```

## Ticket trackers (Jira or Kaneo)

Ticket handling is split three ways: the tracker-neutral workflow (`ticket-sync` agent, managed), one adapter per tracker (`.github/trackers/`, managed), and your project settings (`docs/tickets/config.md`, seed). Set `Provider:` to `jira` or `kaneo` in the config; the agent adds the matching MCP server to `.vscode/mcp.json` if missing. Kaneo has no epics or attachments, so groups become a labelled parent task and design images are linked as repo paths.

**Migrating from the old seeded jira-sync agent:** delete `.github/agents/jira-sync.agent.md`, move your Jira settings into `docs/tickets/config.md`, and rename `.jira-*` files to `docs/tickets/`. `Jira:` story lines are rewritten to `Ticket:` automatically when touched.

## License

[MIT](LICENSE) © Daniel Vella. Use it freely for anything; just keep the copyright notice (a mention is appreciated).

`docs/designs/design.md` is a starter design system: edit it or replace the whole file with your own brand guide. It is a seed file, so package updates never overwrite it, and the designer agent follows whatever it contains.

The design defaults (Inter, Tailwind CSS, Font Awesome) are only named as suggestions in `design.md`; none of their files are bundled, and each remains under its own license.
