O display: flex transforma a caixa num container flex, e tudo o que está dentro dela vira um item flex. No seu caso, o único item é o número.

A partir daí, a caixa pode mandar na posição do número:

Linha	O que faz com o número
justify-content: center	coloca no meio da horizontal ↔
align-items: center	coloca no meio da vertical ↕
Importante: essas duas propriedades só funcionam com display: flex (ou grid). Se você apagar a linha do display, as outras duas param de ter efeito, mesmo continuando no código.

As principais (você vai usar sempre)
Valor	O que significa
block	Ocupa a linha inteira, e o próximo elemento vai para baixo. Aceita width, height, margin e padding. É o padrão de div, p, h1, section.
inline	Fica no meio do texto, ao lado dos vizinhos. Ignora width e height, e a margem de cima/baixo não funciona. É o padrão de span, a, strong.
inline-block	É a mistura dos dois: fica lado a lado como o inline, mas aceita tamanho como o block.
none	O elemento some e não ocupa espaço nenhum, como se não existisse no HTML.
flex	Vira um container flex: organiza os filhos em uma direção, linha (padrão) ou coluna. Por fora, se comporta como block. É o que você usou.
grid	Vira um container grid: organiza os filhos em linhas e colunas ao mesmo tempo, como uma tabela. Por fora, se comporta como block.
As variações "inline" de flex e grid
Valor	O que significa
inline-flex	Por dentro funciona igual ao flex, mas por fora fica lado a lado com os vizinhos, em vez de ocupar a linha inteira.
inline-grid	Mesma ideia, só que com grid.
Isso mostra que o display sempre diz duas coisas:

por fora: como o elemento se comporta com os vizinhos (block ou inline);
por dentro: como ele organiza os filhos (normal, flex ou grid).
Menos comuns (bom saber que existem)
Valor	O que significa
contents	A caixa do elemento "desaparece", mas os filhos continuam na página, como se estivessem direto no avô.
list-item	Faz o elemento se comportar como um li, com marcador (bolinha).
table, table-row, table-cell	Fazem elementos comuns se comportarem como partes de uma tabela. Hoje quase ninguém usa, porque o grid resolve melhor.
flow-root	É um block que "segura" elementos com float dentro dele. Só aparece em layouts antigos com float.
Para guardar
Na prática, 90% do tempo você vai usar block, inline, inline-block, none, flex e grid. Os outros você consulta quando precisar.