# Proposta: Ontologia da Economia de Aldeões do Minecraft

**Disciplina:** Ontologias e Grafos de Conhecimento
**Dupla:** Bruno Lopes Altoé e Vitor Facco Calmon

---

## 1. Definição de escopo

### Porquê

Os aldeões do Minecraft podem comercializar uma grande variedade de itens, de alimentos e ferramentas a blocos decorativos e livros encantados. Diante de um item de que precisa, o jogador costuma se perguntar: **algum aldeão vende esse item? Se vende, qual aldeão, e o que é preciso para conseguir um?**

A disponibilidade de uma troca depende de várias regras combinadas:

- cada item é oferecido apenas por certas profissões, e a profissão depende do bloco de trabalho que o aldeão reivindica;
- as ofertas são desbloqueadas por nível (Novato, Aprendiz, Experiente, Especialista e Mestre), então um item pode existir nas trocas do jogo, mas exigir que o aldeão evolua;
- a cada nível, o aldeão sorteia suas ofertas entre as possíveis para a profissão, então dois aldeões da mesma profissão podem vender itens diferentes;
- alguns aldeões nunca terão profissão (os bobos, ou _nitwits_, e os filhotes), e um zumbi-aldeão só volta a negociar depois de curado.

A pergunta aparece em dois níveis: **no jogo em geral** ("o item X pode ser obtido por troca?") e **em um mundo específico** ("algum dos meus aldeões vende X ou ainda pode vir a vender?").

### Utilidade

A ontologia torna esse conhecimento **explícito, consistente e consultável**. Ela serve de base para:

1. responder, por consulta ao grafo de conhecimento, quais passos levam à obtenção de uma troca de um item desejado;
2. representar o estado de aldeões reais de um mundo salvo e verificar o que cada um oferece ou ainda pode oferecer;
3. servir de vocabulário comum para ferramentas da comunidade, como planejadores de salões de troca.

### Escopo

**Dentro do escopo:**

- Aldeão e seus estados: filhote ou adulto, desempregado, empregado, bobo, zumbi-aldeão.
- Profissão, como papel assumido pelo aldeão ao reivindicar um bloco de trabalho.
- Bloco de trabalho e a profissão associada a ele.
- Nível de profissão e experiência do aldeão.
- Oferta de troca: itens pedidos, item oferecido, limite de usos e reabastecimento.
- Troca, como relação entre jogador, aldeão e oferta.
- Zumbificação e cura, como eventos que mudam a fase do aldeão sem mudar sua identidade.
- Preço das trocas e seus fatores: desconto por cura, demanda, efeito Herói da Vila.

**Fora do escopo:**

- Fabricação (_crafting_), combate, redstone e a mecânica geral de mobs.
- Comerciante ambulante (_wandering trader_).
- Tipo de aldeão por bioma.
- Reprodução de aldeões em detalhe. Só entra o fato de que filhotes não têm profissão.
- Edição Bedrock. A ontologia segue a **Java Edition**.

**Abordagem:** a ontologia de referência será modelada usando Tonto. O OWL será gerado automaticamente a partir dela. O grafo de conhecimento será implementado em Stardog e populado por importação automatizada de dados.

---

## 2. Potenciais usos e usuários

| Usuário | Uso |
| --- | --- |
| Jogadores de sobrevivência avançada | Descobrir se um item pode ser obtido por troca e o que é preciso para conseguir um aldeão que o venda. |
| Administradores de servidores | Inventariar os aldeões de um mundo compartilhado e as trocas disponíveis para os jogadores. |
| Desenvolvedores de ferramentas e _mods_ | Usar a ontologia como vocabulário comum para planejadores, _bots_ e painéis de servidor. |
| Criadores de guias e wikis | Verificar a consistência das regras documentadas e gerar tabelas a partir do grafo. |

---

## 3. Questões de competência

### Disponibilidade de itens

1. O item X pode ser obtido por troca com algum aldeão?
2. Qual profissão, em qual nível, pode oferecer o item X?
3. Quais itens oferecidos por trocas não podem ser obtidos de nenhuma outra forma?

### Profissões e blocos de trabalho

4. Qual bloco de trabalho um aldeão precisa reivindicar para assumir a profissão X?
5. Quais aldeões podem assumir uma profissão e quais nunca podem (bobos e filhotes)?
6. Um aldeão com a profissão X ainda pode mudar de profissão, ou ela já está fixada por uma troca?

### Ofertas e níveis

7. Quais ofertas um aldeão desbloqueia ao atingir o nível Y?
8. Quais profissões compram o item X em troca de esmeraldas?
9. Um aldeão já empregado com a profissão P ainda pode vir a oferecer o item X, considerando os níveis que ainda não desbloqueou?

### Preço

10. Qual é o preço atual da oferta X do aldeão A para o jogador J, considerando descontos e demanda?
11. Quais ofertas do aldeão A ficaram mais baratas para o jogador J depois que J o curou?
12. Qual aldeão de um mundo oferece o item X pelo menor preço?

### Fases e identidade

13. Quais zumbis-aldeões de um mundo eram aldeões com profissão fixada antes da zumbificação?
14. Ao ser curado, um aldeão mantém quais profissões, níveis e ofertas?

### Indivíduos de um mundo

15. Algum aldeão de um mundo vende o item X atualmente?
16. Quais profissões ainda faltam no salão de troca de um mundo para cobrir todos os itens desejados?

---

## 4. Fontes em potencial

### Conhecimento do domínio

1. **Minecraft Wiki** (minecraft.wiki): páginas _Villager_, _Trading_ e _Zombie Villager_, com tabelas de ofertas por profissão e nível, regras de cura e regras de cálculo de preço (demanda, reputação e efeito Herói da Vila). Será a principal referência conceitual;

### Especialistas de domínio

1. **A própria dupla e jogadores experientes**, membros de comunidades técnicas como _r/technicalminecraft_, podem ser consultados para validar as regras e as questões de competência.

### Dados para a A-box

1. **minecraft-data** (PrismarineJS, GitHub): JSONs por versão com itens, entidades e encantamentos. Usado para popular os tipos de item e de aldeão.
2. **Tabelas de troca da Minecraft Wiki**, convertidas em CSV para popular as ofertas por profissão e nível. As trocas não fazem parte dos arquivos de dados do jogo, então essa é a fonte mais completa.
3. **Arquivos de mundo salvo (formato NBT)**, lidos com bibliotecas como `amulet-core`. Desde a versão 1.17, os aldeões ficam nos arquivos de região da pasta `entities/`. Eles contêm aldeões reais com profissão, nível e ofertas. Cada oferta guarda a demanda e o multiplicador de preço, e cada aldeão guarda sua reputação com cada jogador (`Gossips`), inclusive o registro de ter sido curado por ele. Isso permite popular o grafo com **indivíduos**, além dos tipos.
4. **Dados dos jogadores no mundo salvo** (pasta `playerdata/`, também em NBT), que registram os efeitos ativos de cada jogador, como o Herói da Vila. Junto com a reputação, permitem calcular o preço de uma oferta para um jogador específico.

---

## Apêndice A: Declaração de uso de inteligência artificial generativa

Em cumprimento ao art. 19 da Portaria Normativa nº 310, de 23 de outubro de 2026, da Ufes, declaramos o uso de inteligência artificial generativa na elaboração desta proposta.

### Ferramenta e versão

- **Modelo:** Claude Opus 5.5, desenvolvido pela Anthropic, utilizando esforço _Low_ e _Medium_.
- **Ambiente de uso:** assistente Kiro, integrado ao editor Kiro IDE, versão 1.4.121.
- **Configuração:** parâmetros padrão do assistente. Não foram conectadas bases de dados externas.
- **Período de uso:** 6 de novembro de 2026.

### Finalidade e forma de contribuição

A ferramenta foi usada para **brainstorming** e **apoio à redação**:

- **Brainstorming:** explicação das opções do enunciado do trabalho, sugestão de domínios, de escopos e de fontes em potencial, e discussão dos construtos de OntoUML aplicáveis a cada escopo.
- **Redação:** geração do rascunho inicial desta proposta, reformulação de seções a pedido da dupla e revisão da qualidade do texto.

A escolha do domínio, do escopo, do porquê e das questões de competência mantidas foi feita pela dupla.

**Motivação:** acelerar a exploração inicial do domínio e a escrita da proposta, dentro do prazo de entrega.

**Método:** a dupla formulou as instruções, avaliou cada resposta e editou o documento diretamente.

### Revisão e responsabilidade

Todo o conteúdo gerado foi **revisado integralmente pela dupla**. A dupla assume **total responsabilidade** pela precisão, originalidade e adequação das informações apresentadas, conforme o art. 18 da portaria.

Nenhuma autoria ou coautoria é atribuída à ferramenta (art. 17, III). Não foram fornecidos à ferramenta dados pessoais sensíveis nem dados institucionais confidenciais.

### Citação da ferramenta

ANTHROPIC. **Claude Opus 5.5**. [S. l.]: Anthropic, 2026. Modelo de linguagem utilizado por meio do assistente Kiro. Disponível em: <https://kiro.dev/>. Acesso em: 6 nov. 2026.
