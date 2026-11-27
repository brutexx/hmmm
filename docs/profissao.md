# Profissão do aldeão

**Situação:** adotada. O modelo mantém um aviso do Tonto de propósito (ver abaixo).

## Problema

No jogo, a profissão de um aldeão se sustenta de duas formas, uma depois da outra:

- **antes da primeira troca**, pela reivindicação de um bloco de trabalho. Se o bloco é destruído, o aldeão volta a ficar desempregado;
- **depois da primeira troca**, pela própria troca. A profissão fica fixada e o aldeão não a perde mais, nem sem bloco. É por isso que salões de troca às vezes removem os blocos depois de fixar as profissões.

Só um aldeão empregado negocia, então ter a profissão fixada pressupõe estar empregado.

## Alternativas descartadas

- **`role Comerciante` fundado em `relator Troca`, com as profissões abaixo dele.** Exigia que todo açougueiro já tivesse negociado, mas o aldeão vira açougueiro ao reivindicar o defumador, antes de qualquer troca. A troca também não é um relator: é instantânea, e várias trocas entre o mesmo jogador e o mesmo aldeão ficavam indistinguíveis (`RepRel`).
- **`AldeaoEmpregado` fundado diretamente em `Reivindicacao`.** Um aldeão com a profissão fixada que perdesse o bloco deixaria de ser empregado, o que contradiz o jogo.
- **Participação na `Troca` a partir de `AldeaoEmpregado`, com `AldeaoComProfissaoFixada` especializando `Aldeao`.** Diz que o aldeão estava empregado na hora da troca, mas não que continua empregado depois. Gera o aviso `historical-event-source-can-lapse` e exige um papel extra (`AldeaoEmpregadoFixado`) para o genset.

## Solução adotada

Em `src/hmmm.tonto`:

- `type Profissao` é um tipo de ordem superior cujas instâncias são as profissões (`Açougueiro`, `Clérigo`, ...), no padrão `categorization-odell`. O genset `profissão` o declara como `categorizer`.
- `relator Emprego` liga o `AldeaoEmpregado` à sua `Profissao` e funda o papel `AldeaoEmpregado`.
- `relator Reivindicacao` liga o `AldeaoEmpregadoComBloco` ao `BlocoDeTrabalho`.
- `event Troca`, com participações a partir dos tipos rígidos `Aldeao` e `Jogador` (`negociouEm`).
- `historicalRole AldeaoComProfissaoFixada specializes AldeaoEmpregado derivedFrom Aldeao.negociouEm`.
- `complete genset sustentacaoDoEmprego` sobre `AldeaoEmpregado`, com `AldeaoEmpregadoComBloco` e `AldeaoComProfissaoFixada`. Não é `disjoint`: um aldeão fixado pode continuar com o bloco.

Com isso, a estrutura diz:

- todo aldeão empregado reivindica um bloco, ou já negociou, ou os dois;
- um aldeão que nunca negociou e perde o bloco deixa de ser empregado;
- só um aldeão empregado tem a profissão fixada;
- depois de negociar, o aldeão continua empregado para sempre.

## Aviso mantido de propósito

`tonto check` emite `historical-role-specializes-anti-rigid` para `AldeaoComProfissaoFixada`. Um papel histórico vale para sempre depois de começar. Por isso, ao especializar `AldeaoEmpregado`, ele impede que o `Emprego` desse aldeão termine. A regra considera isso coerente e às vezes intencional. Aqui é intencional, porque essa é exatamente a regra do jogo.

## Restrições fora da estrutura

- A `Profissao` ligada ao `Emprego` deve ser a mesma role do aldeão: se o emprego aponta para Açougueiro, o aldeão é `Açougueiro`.
- O bloco reivindicado deve ser o da profissão (defumador para açougueiro, suporte de poções para clérigo). Expressar isso exige tipos de bloco ligados a `Profissao`, ainda não modelados.

## Em aberto

- Se a reivindicação deve ser um evento que cria o `Emprego` (`@creation`).
- Tornar o genset `profissão` `complete` quando as 13 profissões estiverem no modelo.
