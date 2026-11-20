# Tech

## Stack

- **Tonto** (`.tonto`), the OntoUML/UFO textual language from nemo-ufes. Source of truth for the ontology. `AGENTS.md` holds the full language and tooling instructions; follow it.
- **Tonto VS Code extension** (`nemo-ufes.tonto-vscode`) running inside Kiro. It provides the MCP server (`tonto_*` tools) and the Claude Code hook.
- **gUFO / OWL Turtle** (`.ttl`) as the export format.
- **tonto-visualizer** (`src/views/*.tontoview`) for diagram view settings.
- **Stardog** (planned) for the knowledge graph.

No package manager, build system, or test suite. "Building" means validating and exporting the model.

## Tooling access

- Prefer the Tonto MCP tools (`tonto_guide`, `tonto_validate`, `tonto_format`, `tonto_gufo`, ...) when the session exposes them.
- In Kiro they are often not exposed. Then use the globally installed `tonto` CLI (npm `tonto-language`). Do not use `npx` to download it.
- PowerShell may not see `node`/`tonto` on PATH. Refresh first:
  ```powershell
  $env:Path = [Environment]::GetEnvironmentVariable('Path','Machine') + ';' + [Environment]::GetEnvironmentVariable('Path','User')
  ```
- If neither MCP nor CLI works, say validation could not be run and do not call the model valid.

## Common commands

```powershell
tonto guide                                   # read before writing model syntax
tonto patterns [name]                         # validated modeling shapes
tonto explain <stereotype> <stereotype>       # may X specialize Y (exit 0 yes / 1 no)
tonto rules <code>                            # what a diagnostic means
tonto check . --json                          # validate the whole workspace
Get-Content draft.tonto | tonto check - --json --workspace=.   # validate an unsaved draft
tonto check . --anti-patterns                 # advisory review
tonto format src\hmmm.tonto                   # prints canonical layout to stdout (does not write)
tonto ast src\hmmm.tonto                      # parsed model as JSON
tonto gufo src\hmmm.tonto                     # Turtle preview to stdout
tonto gufo src\hmmm.tonto --write             # writes src\hmmm.ttl beside the ontology file
```

To apply formatting safely on Windows PowerShell 5 (no `utf8NoBOM`, console mangles accents):

```powershell
cmd /c "tonto format src\hmmm.tonto > %TEMP%\hmmm.fmt"
git diff --no-index src\hmmm.tonto $env:TEMP\hmmm.fmt
```

## Rules

- After each model edit: format, then validate the workspace. Resolve every diagnostic, warnings included, before calling the model valid.
- Apply `--fix` only for forced repairs, or for a named code when the modeling choice is clear. Never pick between modeling alternatives silently; ask.
- Never hand-edit generated Turtle. Change the `.tonto` source and re-export.
- Do not create `tonto.json` unless an import location needs redirecting.
- Do not commit unless asked. Commit messages are Portuguese, imperative third person (e.g. "Implementa AldeaoSaudavel e AldeaoZumbi").
- `.mcp.json`, `.claude/`, `.codex/` are local, machine-specific config and are gitignored.
