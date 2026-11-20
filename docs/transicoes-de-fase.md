# Transições entre fases do aldeão

**Situação:** em avaliação. Por enquanto o modelo só documenta a restrição.

## Problema

Em `src/hmmm.tonto`, o `kind Aldeao` tem duas partições de fases:

- `estagiosDaVida`: `AldeaoBebe` e `AldeaoAdulto`;
- `saúde`: `AldeaoSaudavel` e `AldeaoZumbi`.

As duas partições se comportam de forma diferente no jogo:

- um bebê se torna adulto, mas um adulto nunca volta a ser bebê;
- um aldeão saudável pode virar zumbi e um zumbi pode voltar a ser saudável, quantas vezes for.

Uma `phase` em Tonto diz apenas que o aldeão pode entrar e sair dela (anti-rigidez). Ela não diz em que ordem as fases se sucedem nem se a mudança pode ser desfeita. Por isso as duas partições, tal como estão escritas, afirmam a mesma coisa, e a diferença entre elas não aparece no modelo.

O Tonto não tem um construto de transição entre fases (uma máquina de estados). "Um adulto não volta a ser bebê" é uma restrição dinâmica, que nenhuma declaração estrutural impõe.

## Solução adotada por enquanto

A restrição está escrita na documentação `///` de `AldeaoAdulto`:

```tonto
/// Um aldeão que já cresceu.
///
/// Um aldeão adulto não volta a ser bebê.
phase AldeaoAdulto specializes Aldeao
```

O texto aparece no hover do editor e vai para a exportação gUFO como comentário, mas nenhuma verificação o lê.

## Solução possível: as mudanças como eventos

No UFO, uma mudança de fase acontece por causa de um evento que altera o aldeão. O Tonto tem o estereótipo `@change` (endurant → perdurant) para essa ligação. A proposta é declarar um evento para cada mudança e usar a cardinalidade para distinguir os dois casos:

```tonto
/// Um aldeão bebê cresceu e tornou-se adulto.
event Crescimento

/// Um aldeão saudável foi infectado e tornou-se zumbi.
event Infeccao

/// Um aldeão zumbi foi curado e voltou a ser saudável.
event Cura

@change relation Aldeao (const) [1] -- cresceuEm -- [0..1] Crescimento
@change relation Aldeao (const) [1] -- foiInfectadoEm -- [0..*] Infeccao
@change relation Aldeao (const) [1] -- foiCuradoEm -- [0..*] Cura
```

O trecho foi validado com `tonto check` 0.9.4 junto com as fases atuais, sem erros nem avisos. O `(const)` é exigido pela regra `immutable-end-not-const`: um evento já ocorreu, então o aldeão que ele mudou não pode ser trocado.

Como ler:

- cada evento muda exatamente um aldeão (`[1]` do lado de `Aldeao`);
- cada aldeão cresce no máximo uma vez (`[0..1]` do lado de `Crescimento`);
- cada aldeão pode ser infectado e curado várias vezes (`[0..*]`).

### O que a proposta resolve

- A irreversibilidade do crescimento e a repetição do ciclo saudável/zumbi ficam explícitas no modelo.
- A proposta já prevê os eventos de zumbificação e cura, que estão no escopo da proposta (`docs/proposta.md`, seção 1).
- A proposta dá onde ancorar os fatos ligados à cura, como o desconto que o jogador que curou recebe (questão de competência 11).

### Limitações

- A proposta não impõe a restrição. O modelo não proíbe um evento que leve de adulto a bebê, apenas não declara nenhum, e o `[0..1]` limita quantos crescimentos há sem dizer de qual fase para qual.
- Na exportação gUFO, `@change` não tem equivalente e gera a informação `gufo-no-counterpart`. A relação sai como propriedade OWL própria, sem alinhamento a uma propriedade do gUFO.
- Impor a restrição de fato exigiria uma linguagem de restrições externa, como OCL, ou formas SHACL sobre a exportação.

### Alternativa descartada

`historicalRole AldeaoAdulto specializes Aldeao derivedFrom Crescimento` faria "adulto" valer para sempre depois do crescimento, o que captura a irreversibilidade. Porém isso exige que todo adulto tenha passado por um `Crescimento`, e no jogo há aldeões que já surgem adultos (gerados com a vila, por exemplo). Também tiraria `AldeaoAdulto` da partição de fases `estagiosDaVida`, que só admite `phase`.

### Pontos a decidir antes de adotar

- Se `Crescimento` deve ter participantes além do aldeão (nenhum, no jogo).
- Se `Cura` deve ligar o jogador que a fez, com um `@participation`, para sustentar o desconto por cura.
- Se `Infeccao` deve ligar o zumbi que a causou.
