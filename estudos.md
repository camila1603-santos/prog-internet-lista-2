# Estudos de CSS: a propriedade `display`

## 1. O que o `display` define

O `display` sempre diz **duas coisas** sobre um elemento:

- **Por fora:** como o elemento se comporta com os vizinhos (`block` ou `inline`).
- **Por dentro:** como ele organiza os filhos (normal, `flex` ou `grid`).

---

## 2. Valores principais (você vai usar sempre)

| Valor | O que significa |
|---|---|
| `block` | Ocupa a linha inteira, e o próximo elemento vai para baixo. Aceita `width`, `height`, `margin` e `padding`. É o padrão de `div`, `p`, `h1` e `section`. |
| `inline` | Fica no meio do texto, ao lado dos vizinhos. Ignora `width` e `height`, e a margem de cima/baixo não funciona. É o padrão de `span`, `a` e `strong`. |
| `inline-block` | Mistura dos dois: fica lado a lado como o `inline`, mas aceita tamanho como o `block`. |
| `none` | O elemento some e não ocupa espaço nenhum, como se não existisse no HTML. |
| `flex` | Vira um container flex: organiza os filhos em uma direção, linha (padrão) ou coluna. Por fora, se comporta como `block`. |
| `grid` | Vira um container grid: organiza os filhos em linhas e colunas ao mesmo tempo, como uma tabela. Por fora, se comporta como `block`. |

---

## 3. Variações "inline" de flex e grid

| Valor | O que significa |
|---|---|
| `inline-flex` | Por dentro funciona igual ao `flex`, mas por fora fica lado a lado com os vizinhos, em vez de ocupar a linha inteira. |
| `inline-grid` | Mesma ideia, só que com `grid`. |

---

## 4. Valores menos comuns (bom saber que existem)

| Valor | O que significa |
|---|---|
| `contents` | A caixa do elemento "desaparece", mas os filhos continuam na página, como se estivessem direto no avô. |
| `list-item` | Faz o elemento se comportar como um `li`, com marcador (bolinha). |
| `table`, `table-row`, `table-cell` | Fazem elementos comuns se comportarem como partes de uma tabela. Hoje quase ninguém usa, porque o `grid` resolve melhor. |
| `flow-root` | É um `block` que "segura" elementos com `float` dentro dele. Só aparece em layouts antigos com `float`. |

---

## 5. Na prática: centralizando com `display: flex`

O `display: flex` transforma a caixa num **container flex**, e tudo o que está dentro dela vira um **item flex**.

A partir daí, a caixa controla a posição dos filhos:

| Propriedade | O que faz com os filhos |
|---|---|
| `justify-content: center` | coloca no meio da horizontal ↔ |
| `align-items: center` | coloca no meio da vertical ↕ |

Exemplo (uma caixa com um número centralizado dentro):

```css
.box {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

> ⚠️ **Importante:** `justify-content` e `align-items` só funcionam com `display: flex` (ou `grid`). Se você apagar a linha do `display`, as outras duas param de ter efeito, mesmo continuando no código.

---

## 6. Propriedades do elemento × propriedades que organizam os filhos

Existem **dois tipos** de propriedades em CSS.

### Tipo 1: propriedades que falam do próprio elemento

Mudam **só o elemento onde você escreve**:

- `width`, `height`
- `background-color`
- `color`, `font-size`
- `border`, `padding`, `margin`

Exemplo: `background-color: blue` no `#box1` deixa só o box1 azul.

### Tipo 2: propriedades que o pai usa para organizar os filhos

Você escreve no **pai**, mas o efeito aparece na **posição dos filhos**:

| Propriedade | O que faz | Funciona com |
|---|---|---|
| `display: flex` | liga o "modo organizador" | — |
| `justify-content` | controla a posição dos filhos na horizontal | flex e grid |
| `align-items` | controla a posição dos filhos na vertical | flex e grid |
| `flex-direction` | define se os filhos ficam em linha ou em coluna | só flex |
| `gap` | define o espaço entre os filhos | flex e grid |
| `flex-wrap` | deixa os filhos quebrarem para a linha de baixo quando não cabem (`wrap`). O padrão (`nowrap`) espreme todos numa linha só. | só flex |
| `align-content` | alinha as **linhas** entre si, quando há mais de uma (com `flex-wrap: wrap`) | flex e grid |

> ⚠️ Essas propriedades só funcionam junto com `display: flex` (ou `grid`, quando a tabela indica). Sem ele, `justify-content` e `align-items` não fazem nada.

> 💡 "Horizontal" e "vertical" valem para o padrão (`flex-direction: row`). Com `flex-direction: column` eles **trocam**: `justify-content` passa a controlar a vertical e `align-items` a horizontal.

### Tipo 2 (versão filho): propriedades que vão no item flex

Algumas propriedades são escritas no **filho**, mas também só funcionam se o **pai** tiver `display: flex`. Elas servem para um filho "pedir" algo diferente dos outros:

| Propriedade | O que faz | Funciona com |
|---|---|---|
| `align-self` | alinha **só esse filho**, ignorando o `align-items` do pai | flex e grid |
| `flex-grow` | quanto o filho **cresce** para ocupar o espaço que sobra (`0` = não cresce, `1` = cresce) | só flex |
| `flex-shrink` | quanto o filho **encolhe** quando falta espaço (`0` = não encolhe) | só flex |
| `flex-basis` | tamanho inicial do filho, antes de crescer ou encolher | só flex |
| `flex` | atalho para os três acima. `flex: 1` = "divida o espaço igualmente com os irmãos" | só flex |
| `order` | muda a **ordem na tela** sem mexer no HTML (o menor número vem primeiro) | flex e grid |

Exemplo: no chat da atividade 7, para jogar só o nome "Eu" para a direita, sem mexer nos outros:

```css
.container {
    display: flex;
    flex-direction: column;
}

.eu {
    align-self: flex-end;   /* só esse filho vai para o fim (direita, porque é coluna) */
}
```

> ⚠️ Se o pai **não** tiver `display: flex`, essas propriedades no filho são ignoradas. É o inverso do `float`, que é ignorado justamente quando o pai **é** flex (ver seção 8).

### Comparação: a estante e os livros

- A **estante** (pai) decide: "os livros ficam em pé, um ao lado do outro, centralizados". Essas são as regras de organização (**tipo 2**).
- Cada **livro** (filho) tem o próprio tamanho e a própria cor (**tipo 1**).

A estante não define o tamanho dos livros, ela só os arruma.

### Exemplo: atividade 6c

```html
<div class="container">
    <div class="box" id="box1">1</div>
    <div class="box" id="box2">2</div>
    <div class="box" id="box3">3</div>
    <div class="box" id="box4">4</div>
</div>
```

```css
.container {
    display: flex;              /* tipo 2: arruma os filhos (as boxes) lado a lado */
}

.box {
    display: flex;              /* tipo 2: arruma o filho DELA (o número) */
    justify-content: center;    /* tipo 2: centraliza o número na horizontal */
    align-items: center;        /* tipo 2: centraliza o número na vertical */
    height: 150px;              /* tipo 1: tamanho da própria box */
    width: 25%;                 /* tipo 1: tamanho da própria box */
}
```

O `.box` é ao mesmo tempo **filho** do container e **pai** do número que está dentro dele. Por isso tem os dois tipos de propriedade:

- o **tipo 1** define o tamanho dele;
- o **tipo 2** organiza o número que está dentro dele.

### Pergunta para testar

Ao escrever uma propriedade, pergunte: **"isso muda o elemento ou a posição do que está dentro dele?"**

- Muda o elemento → **tipo 1**.
- Muda a posição do que está dentro → **tipo 2**, e precisa de `display: flex`.

---

## 7. `position` × `display`

### Qual a diferença?

| | `display` | `position` |
|---|---|---|
| Pergunta que responde | "Como este elemento ocupa espaço e como organiza os filhos?" | "Este elemento fica onde o fluxo normal colocar ou eu escolho onde ele fica?" |
| Quem decide o lugar | O **pai** organiza os filhos (com flex, grid...) | O **próprio elemento** diz onde quer ficar (`top`, `right`, `bottom`, `left`) |
| Tipo de layout | Organização em grupo: linhas, colunas, distribuição de espaço | Posição exata: cantos, sobreposição, elementos "flutuando" |

Comparação com a estante:

- **`display`** é a estante arrumando os livros.
- **`position`** é tirar um livro da estante e colocá-lo num lugar exato, por exemplo "em cima da mesa, encostado no canto esquerdo".

### Como eles se relacionam?

São propriedades diferentes, mas uma interfere na outra:

1. **`absolute` e `fixed` tiram o elemento do fluxo.** O pai deixa de organizá-lo. Se o container tem `display: flex`, um filho com `position: absolute` **ignora** o flex, e os outros filhos se arrumam como se ele não existisse.
2. **`absolute` e `fixed` fazem o elemento se comportar como `block` por fora.** Um `span` (que é `inline`) com `position: absolute` passa a aceitar `width` e `height`.
3. **`display: none` vence tudo.** O elemento some, não importa o `position` que ele tenha.
4. **Dá para usar os dois juntos.** O `display` de um elemento continua organizando os filhos **dele**. Uma box com `position: absolute` pode ter `display: flex` para centralizar o texto que está dentro dela.

### Os valores de `position`

| Valor | Sai do fluxo? | Referência para `top`/`left`... | Característica |
|---|---|---|---|
| `static` | Não | Nenhuma (`top`, `left` etc. **não funcionam**) | É o **padrão** de todo elemento. Fica onde o fluxo normal colocar. |
| `relative` | Não | **A posição original** do próprio elemento | O elemento continua ocupando o espaço original. `top: 20px` só o desloca 20px para baixo de onde ele estaria. Muito usado **no pai**, para virar referência dos filhos `absolute`. |
| `absolute` | **Sim** | O **ancestral posicionado mais próximo** (qualquer `position` diferente de `static`). Se não houver nenhum, usa a página. | Não ocupa espaço: os vizinhos agem como se ele não existisse. Ideal para colocar algo num canto ou no centro de um container. |
| `fixed` | **Sim** | A **janela do navegador** | Fica parado na tela mesmo quando você rola a página. Usado em menus fixos, botões de "voltar ao topo", banners. |
| `sticky` | Não (até "grudar") | O **container onde ele está rolando** | Mistura de `relative` e `fixed`: se comporta normalmente até a rolagem chegar no valor de `top`, e aí "gruda" ali. Usado em cabeçalhos de tabela e menus que grudam no topo. |

> ⚠️ `top`, `right`, `bottom` e `left` só funcionam quando o `position` **não é** `static`.

> 💡 Quando elementos posicionados se sobrepõem, `z-index` define quem fica na frente (o número maior fica por cima).

### A dupla mais usada: pai `relative` + filho `absolute`

```css
.container {
    position: relative;   /* vira a referência, mas continua no lugar dele */
}

.box {
    position: absolute;   /* se posiciona em relação ao .container */
    top: 0px;
    right: 0px;           /* canto de cima à direita DO CONTAINER */
}
```

Sem o `position: relative` no pai, o `absolute` procura um ancestral posicionado, não encontra e usa a **página inteira** como referência.

### Exemplo: atividade 6c

Quatro boxes nos cantos de um quadrado e uma no centro:

```css
.container {
    width: 600px;
    height: 600px;
    position: relative;
}

.box {
    width: 150px;
    height: 150px;
}

#box1 { position: absolute; top: 0px;    left: 0px;  }   /* canto superior esquerdo */
#box2 { position: absolute; top: 0px;    right: 0px; }   /* canto superior direito */
#box3 { position: absolute; bottom: 0px; right: 0px; }   /* canto inferior direito */
#box4 { position: absolute; bottom: 0px; left: 0px;  }   /* canto inferior esquerdo */
#box5 { position: absolute; top: 225px;  left: 225px; }  /* centro */
```

Conta para centralizar a box5:

```
(tamanho do container − tamanho da box) ÷ 2
(600 − 150) ÷ 2 = 225px
```

### Quando usar cada um?

- Vários elementos **em linha, coluna ou grade**, com o espaço distribuído entre eles → **`display`** (flex ou grid).
- Um elemento num **lugar exato** (canto, centro, por cima de outro) → **`position`**.
- Elemento que **acompanha a rolagem** → `fixed` ou `sticky`.

---

## 8. `float` e `clear`

### O que o `float` faz

O `float` empurra o elemento para um dos lados (esquerda ou direita) e deixa **o conteúdo seguinte contornar ele**, como uma foto no meio de uma matéria de jornal com o texto em volta.

| Valor | O que significa |
|---|---|
| `left` / `inline-start` | Flutua para a esquerda (início da linha) |
| `right` / `inline-end` | Flutua para a direita (fim da linha) |
| `none` | Padrão: não flutua |

> 💡 `inline-start` e `inline-end` são as versões "modernas" de `left` e `right`. Em português (texto da esquerda para a direita), dá no mesmo.

O elemento com `float` sai **parcialmente** do fluxo:

- os elementos `block` que vêm depois **sobem** e ocupam o lugar dele, como se ele não existisse;
- mas o **texto** desses elementos desvia do float e fica ao lado dele.

Por isso, quando o `float` está funcionando, o próximo elemento aparece **do lado** do flutuado, e não embaixo.

### O que o `clear` faz

O `clear` vai no elemento que vem **depois** do float e diz: **"não fique ao lado de nenhum float, comece abaixo dele"**.

| Valor | O que significa |
|---|---|
| `left` | Desce abaixo dos floats da esquerda |
| `right` | Desce abaixo dos floats da direita |
| `both` | Desce abaixo de qualquer float, dos dois lados (o mais usado) |
| `none` | Padrão: pode ficar ao lado dos floats |

Comparação com o jornal: o `float` é a foto encostada no canto, e o `clear: both` é um parágrafo que diz "eu só começo depois que a foto acabar".

### Cuidado 1: `float` não funciona em filho de flex

Se o pai tem `display: flex` (ou `grid`), o `float` dos filhos **é ignorado**. A sintaxe está certa, mas não tem efeito nenhum.

A solução é colocar o elemento dentro de uma `div` comum. Essa `div` passa a ser o filho do flex, e o elemento flutuado fica dentro dela, onde o `float` volta a funcionar.

### Cuidado 2: o pai "não enxerga" o float

Se uma `div` só tem filhos com `float`, a altura dela fica **zero**, porque o float não conta para o tamanho do pai. Formas de resolver:

- colocar depois do float um elemento com `clear: both` (que é o caso da atividade 7);
- ou usar `display: flow-root` no pai (ver seção 4).

### Exemplo: atividade 7 (chat)

```html
<div class="container">          <!-- display: flex -->
    <div class="msg">            <!-- filho do flex, div comum -->
        <span class="nome">Pedro Silva</span>
        <div class="box" id="box1">Olá, tudo bem?</div>
    </div>
    ...
</div>
```

```css
.container {
    display: flex;              /* organiza as .msg em coluna */
    flex-direction: column;
}

.nome {
    float: inline-start;        /* funciona porque o pai é a .msg, não o container */
}

.box {
    clear: both;                /* o balão começa ABAIXO do nome, e não ao lado */
}
```

O que acontece em cada caso:

| Situação | Resultado na tela |
|---|---|
| `.nome` direto dentro do `.container` (sem a `.msg`) | O `float` é ignorado, porque o pai é flex |
| Com a `.msg`, mas sem `clear` no `.box` | O nome fica à esquerda e o balão **ao lado** dele |
| Com a `.msg` e `clear: both` no `.box` | O nome fica em cima e o balão **embaixo** ✅ |

### Quando usar `float` hoje?

Hoje o `float` é usado principalmente para **texto contornando uma imagem**. Para montar layout (colunas, alinhar à esquerda ou à direita), o `flex` e o `grid` resolvem melhor. Mesmo assim, vale entender o `float`, porque ele aparece em código antigo e em exercícios.

---

## 📌 Para guardar

- Na prática, **90% do tempo** você vai usar `block`, `inline`, `inline-block`, `none`, `flex` e `grid`. Os outros você consulta quando precisar.
- O `display` define o comportamento **por fora** (com os vizinhos) e **por dentro** (com os filhos).
- `justify-content` e `align-items` **dependem** de `display: flex` ou `grid`.
- **Tipo 1** (`width`, `height`, `background-color`...) muda o próprio elemento. **Tipo 2** (`justify-content`, `align-items`, `gap`...) vai no pai e organiza os filhos.
- Um elemento pode ser pai e filho ao mesmo tempo, então pode ter os dois tipos de propriedade.
- `align-self`, `flex-grow`, `flex-shrink`, `flex` e `order` vão no **filho**, mas só funcionam se o **pai** tiver `display: flex`.
- `flex-direction`, `flex-wrap` e `flex-*` são **só do flex**. `justify-content`, `align-items`, `gap` e `align-self` funcionam também no `grid`.
- **`display`**: o pai organiza os filhos. **`position`**: o elemento escolhe onde fica.
- `absolute` e `fixed` **tiram o elemento do fluxo**, e o `display: flex` do pai deixa de afetá-lo.
- `absolute` usa como referência o ancestral posicionado mais próximo. Por isso o pai costuma levar `position: relative`.
- `float` empurra o elemento para um lado e faz o conteúdo seguinte ficar **ao lado** dele. `clear: both` faz o elemento seguinte começar **abaixo** do float.
- `float` **não funciona** em filho direto de `display: flex` ou `grid`. Para usar, coloque o elemento dentro de uma `div` comum.
