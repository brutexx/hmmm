# Structure

```
AGENTS.md                 Tonto agent instructions (language, CLI, MCP, common mistakes). Authoritative.
CLAUDE.md                 Imports AGENTS.md for Claude Code.
README.md                 Placeholder.
hmmm.ttl                  OWL Turtle (Protégé-style, namespace http://www.semanticweb.org/bruno-vitor/ontologies/2026/11/hmmm/).
                          Not where `tonto gufo --write` puts its output. Ask before overwriting or deleting it.
docs/
  proposta.md             Project proposal: scope, users, competency questions, data sources, AI-use declaration.
  transicoes-de-fase.md   Open design question: phase transitions (baby→adult irreversible, healthy↔zombie reversible).
src/
  hmmm.tonto              One-file ontology `hmmm`. All model declarations live here.
  views/hmmm.tontoview    tonto-visualizer diagram settings (JSON).
.kiro/steering/           These steering files.
.mcp.json, .claude/, .codex/   Local Tonto MCP/hook config (gitignored).
```

## Model conventions

- `src/hmmm.tonto` is a one-file ontology. If it grows, split it or add packages with `tonto new package` / `tonto split`, never by hand.
- Class names are Portuguese PascalCase without accents (`Aldeao`, `AldeaoBebe`). Generalization set names are camelCase and may carry accents (`estagiosDaVida`, `saúde`).
- Phases of `Aldeao` are grouped in `disjoint complete genset`s, one per partition (life stage, health).
- Document declarations with `///` in Portuguese, describing the domain (what a villager is, what happened), not data storage.
- Constraints Tonto cannot express (e.g. "an adult never becomes a baby again") go in the `///` documentation of the affected class, and the design discussion goes in `docs/`.

## Docs conventions

- Design notes and unresolved modeling questions go in `docs/` as Portuguese Markdown, one file per topic, with a status line ("**Situação:** ...").
- `docs/proposta.md` is a submitted deliverable. Edit it only when asked.
