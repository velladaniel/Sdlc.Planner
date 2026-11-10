# Sdlc.Planner

NuGet package that delivers the Sdlc.Planner AI harness (Copilot agents, instructions and docs templates) into any repo, and keeps it up to date when the package version changes.

## What gets installed

| Kind | Files | Behaviour |
|---|---|---|
| **Managed** | `.github/agents/{business-analyst,architect,designer,product-owner,automation-qa,proposals}.agent.md`, `.github/instructions/*.instructions.md` | Overwritten on build when the package changes. Don't edit in the consuming repo. |
| **Seed** | `.github/copilot-instructions.md`, `.github/agents/jira-sync.agent.md`, `.vscode/mcp.json`, `docs/{readme,pid,project-overview,architecture}.md`, `docs/proposals/{readme,_template}.md`, `docs/{technical-decisions,risk-log}/{readme,_template}.md`, `docs/reqs/{readme,_template}.md`, `docs/designs/design.md` | Copied only if missing, so project-specific content is never overwritten. Decisions and risks are one date-named file per entry (no sequential numbers, no index) so feature branches never conflict. jira-sync, mcp.json and design.md contain placeholders to fill in. |

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
