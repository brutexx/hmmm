# Product

**hmmm** is an ontology of the Minecraft (Java Edition) villager economy, built by Bruno Lopes Altoé and Vitor Facco Calmon for the course "Ontologias e Grafos de Conhecimento" (Ufes).

It answers: can item X be obtained by trading with a villager, which villager (profession, level) offers it, and what does it take to get one? It works at two levels: the game in general, and the villagers of one specific saved world.

## Scope

- In: villager and its phases (baby/adult, healthy/zombie, unemployed, employed, nitwit), profession as a role founded on claiming a workstation block, profession level and experience, trade offers (wanted items, offered item, max uses, restock), trade as a relation between player, villager and offer, zombification and curing as events, price factors (cure discount, demand, Hero of the Village).
- Out: crafting, combat, redstone, general mob mechanics, wandering trader, biome villager types, breeding details, Bedrock Edition.

## Pipeline

1. Reference ontology modeled in Tonto (OntoUML/UFO), `src/hmmm.tonto`.
2. OWL/gUFO generated from it.
3. Knowledge graph in Stardog, populated from minecraft-data (PrismarineJS), Minecraft Wiki trade tables (CSV), and NBT world saves (`entities/`, `playerdata/`, read with e.g. `amulet-core`).

The competency questions in `docs/proposta.md` (section 3) define what the model must be able to answer. Check new modeling choices against them.

## Language

Domain content is in Brazilian Portuguese: class names (`Aldeao`, `AldeaoZumbi`), `///` documentation, docs, and commit messages. Keep it that way.
