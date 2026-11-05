# Estudos de CSS

Material de estudo da Lista de Atividades de CSS (seletores, display, position e animação). Cada assunto tem explicação, exemplos e a ligação com as atividades onde ele apareceu. Sempre que possível, os exemplos usam os seus próprios códigos.

**Como ler os avisos:**

- 🧩 **Na sua atividade:** onde o assunto apareceu nos seus exercícios.
- ⚠️ **Cuidado:** erro comum. Vários deles já apareceram nos seus códigos.
- 💡 **Dica:** truque ou atalho útil.
- 🔗 **Conexão:** ligação com outro assunto deste material.

## Índice

1. Fundamentos: regra CSS e como incluir CSS no HTML
2. Seletores, cascata e especificidade
3. Texto, cores e herança
4. Tamanhos, unidades e o modelo de caixa
5. `display` e flexbox
6. `position` e `z-index`
7. `float` e `clear`
8. Fundos, gradientes e formas
9. Pseudo-elementos `::before` e `::after`
10. Sombras: `box-shadow` e `text-shadow`
11. Animação: `transform`, `@keyframes` e `animation`
12. Erros que já apareceram e como investigar
13. Teste seus conhecimentos
14. Resumo para revisão

---

## 1. Fundamentos

### 1.1 O que é CSS

- O **HTML** diz **o que** cada coisa é: um título, um parágrafo, uma lista, uma caixa.
- O **CSS** diz **como** cada coisa aparece: cor, tamanho, posição, animação.

Comparação: o HTML é a casa construída (paredes, portas, janelas); o CSS é a pintura, os móveis e a decoração. Dá para trocar toda a decoração sem derrubar nenhuma parede.

CSS significa *Cascading Style Sheets* (folhas de estilo em cascata). A palavra "cascata" é importante e aparece na seção 2.

### 1.2 Anatomia de uma regra CSS (atividade 1)

```css
p {
    color: blue;
    font-size: 20px;
}
```

```
 p  {  color: blue;  font-size: 20px;  }
 │     └────┬────┘   └──────┬──────┘
 │      declaração       declaração
 │
 seletor           { ... } = bloco de declarações
```

| Parte | O que é | No exemplo |
|---|---|---|
| **Regra** | o conjunto inteiro (seletor + bloco) | tudo acima |
| **Seletor** | quem vai receber o estilo | `p` |
| **Declaração** | uma instrução, no formato `propriedade: valor;` | `color: blue;` |
| **Propriedade** | o que vai mudar | `color` |
| **Valor** | como vai ficar | `blue` |

Regras de escrita:

- Cada declaração termina com `;`, e o `:` separa a propriedade do valor.
- Espaços e quebras de linha entre as declarações não importam: `p{color:blue}` funciona igual. A exceção é **dentro do seletor**, onde o espaço tem significado (seção 2.2).
- Comentários em CSS são `/* assim */`, como os `/*regra 1*/` da atividade 4. O `//` **não** é comentário em CSS, e o `<!-- -->` é comentário de HTML.

> ⚠️ **O CSS não mostra erro na tela.** Quando o navegador não entende uma declaração, ele simplesmente a ignora e segue em frente. Por isso, quando "nada acontece", quase sempre é um erro de digitação, um `;` ou `}` faltando, ou um seletor errado.
>
> Exemplo: sem o `;` depois de `red`, o navegador lê `color: red font-size: 20px`, não entende, e descarta **as duas** declarações.
>
> ```css
> p {
>     color: red         /* faltou o ; */
>     font-size: 20px;   /* esta também é perdida */
> }
> ```

### 1.3 Três formas de colocar CSS no HTML (atividade 3)

| Forma | Onde fica | Vale para | Quando usar |
|---|---|---|---|
| **Inline** | no atributo `style` da tag | só aquele elemento | quase nunca; testes rápidos |
| **Interno** | na tag `<style>`, dentro do `<head>` | só aquela página | exercícios e páginas únicas (foi o que você usou em todas as atividades) |
| **Externo** | arquivo `.css` ligado pela tag `<link>` | todas as páginas que tiverem o `<link>` | sites de verdade (o mais recomendado) |

```html
<!-- 1. Inline -->
<p style="color: blue;">Texto</p>

<!-- 2. Interno -->
<head>
    <style>
        p { color: blue; }
    </style>
</head>

<!-- 3. Externo -->
<head>
    <link rel="stylesheet" href="estilo.css">
</head>
```

🔗 **Conexão:** quando as formas disputam o mesmo elemento, o **inline** ganha das outras duas (seção 2.4). Entre interno e externo, ganha quem aparecer **depois** no HTML.

---

## 2. Seletores, cascata e especificidade

### 2.1 Seletores básicos

| Seletor | Exemplo | Seleciona | Na sua atividade |
|---|---|---|---|
| Tipo (tag) | `li` | todos os `<li>` | atividade 4, regra 1 |
| Classe | `.fruta` | todos com `class="fruta"` | atividades 4, 8, 9 e 10 |
| Id | `#lip2` | o elemento com `id="lip2"` | atividades 2, 4 e 9 |
| Universal | `*` | todos os elementos | atividade 9 (borda em tudo para enxergar as caixas) |
| Atributo | `[draggable="true"]` | elementos com esse atributo e esse valor | atividade 4, regra 5 |
| Pseudo-elemento | `.mar::before` | uma parte "extra" criada pelo CSS | atividade 10 (seção 9) |

**Classe × id**

| | Classe (`.`) | Id (`#`) |
|---|---|---|
| No HTML | `class="faixa preta"` | `id="japao"` |
| Pode repetir? | sim, vários elementos podem ter a mesma classe | não, o id é **único** na página |
| Vários no mesmo elemento? | sim, separados por espaço | não, um só |
| Força (seção 2.4) | média | alta |

💡 Um elemento pode ter **várias classes**. Na atividade 8, `class="faixa preta"` dá ao elemento as classes `faixa` **e** `preta`. Assim, `.faixa` define o que todas as faixas têm em comum (a altura), e `.preta` define só a cor.

💡 **Bom saber: pseudo-classe.** O `:hover` (com **um** `:`) aplica o estilo só quando o mouse está em cima do elemento: `a:hover { color: red; }`. Não confunda com pseudo-elemento (`::before`, com **dois** `:`), que cria uma parte nova (seção 9).

### 2.2 Combinando seletores (atividades 4 e 8)

| Escrita | Nome | Significado | Exemplo |
|---|---|---|---|
| `A, B` | grupo (vírgula) | aplica em A **e** em B | `#lip1, #lip2, #lip3` e `.mar::before, .mar::after` |
| `AB` (colado) | composto | **um mesmo elemento** que é A e B ao mesmo tempo | `#japao.bandeira`, `li.churras#lip5` |
| `A B` (espaço) | descendente | B que está **dentro** de A, em qualquer nível | `.input button` (atividade 7) |
| `A > B` | filho direto | B que é filho **imediato** de A | `#japao.bandeira > div.circulo` |
| `A + B` | irmão adjacente | B que vem **logo depois** de A, com o mesmo pai | `li + li.churras` |
| `A ~ B` | irmão geral | B que vem depois de A (não precisa ser logo depois), com o mesmo pai | (bom saber) |

**Descendente × filho direto**

```html
<div class="a">
    <p>filho</p>
    <div>
        <p>neto</p>
    </div>
</div>
```

- `.a p` → pega o **filho e o neto** (qualquer nível dentro).
- `.a > p` → pega **só o filho**.

> ⚠️ **Um espaço muda tudo.**
> - `#japao.bandeira` (colado) = o elemento que tem id `japao` **e** classe `bandeira`.
> - `#japao .bandeira` (com espaço) = um elemento com classe `bandeira` **dentro** do `#japao`.

💡 **Leia seletores da direita para a esquerda.** A parte da direita é quem recebe o estilo; o resto diz onde ele precisa estar.

`#brasil.bandeira > div.circulo > div.faixa` = "a `div.faixa`, que é filha direta da `div.circulo`, que é filha direta do elemento com id `brasil` e classe `bandeira`".

🔗 **Conexão com o HTML: âncoras internas (atividade 8).** O `#` do link é o mesmo `#` do seletor de id:

```html
<a href="#japao">Bandeira Japão</a>   <!-- ao clicar, a página rola até... -->
<div class="bandeira" id="japao">     <!-- ...o elemento com id="japao" -->
```

O mesmo `id` serve para o CSS (`#japao`) e para o link (`href="#japao"`).

### 2.3 A cascata: quem ganha quando duas regras disputam

Muitas vezes, duas ou mais regras tentam mudar **a mesma propriedade** do **mesmo elemento**. O navegador decide quem ganha nesta ordem:

1. **Especificidade:** o seletor mais específico (mais "forte") ganha.
2. **Ordem:** se a força for igual, ganha a regra escrita **por último**.

Existe também o `!important`, que passa na frente de tudo, mas ele bagunça a cascata e é melhor evitar.

⚠️ A regra da ordem vale também **dentro da mesma regra**: se você escrever a mesma propriedade duas vezes, só a última vale. Foi o que aconteceu com o `animation` duplicado da atividade 10 (seção 11.5).

### 2.4 Calculando a especificidade

Conte três "colunas" no seletor:

| Coluna | O que conta | Força |
|---|---|---|
| **A** | ids (`#lip2`) | alta |
| **B** | classes (`.fruta`), atributos (`[draggable="true"]`) e pseudo-classes (`:hover`) | média |
| **C** | tags (`li`, `ol`) e pseudo-elementos (`::before`) | baixa |

O `*` e os combinadores (espaço, `>`, `+`, `~`) não contam.

Compare **coluna por coluna, da esquerda para a direita**: quem tiver o número maior na primeira coluna diferente ganha. Funciona como comparar números: 1 id vence qualquer quantidade de classes. E o `style="..."` inline ganha de qualquer seletor.

**As regras da atividade 4:**

| Regra | Seletor | A (ids) | B (classes/atributos) | C (tags) | Pontuação |
|---|---|---|---|---|---|
| 1 | `li` | 0 | 0 | 1 | (0,0,1) |
| 2 | `.churras` | 0 | 1 | 0 | (0,1,0) |
| 3 | `.fruta` | 0 | 1 | 0 | (0,1,0) |
| 4 | `li+li.churras` | 0 | 1 | 2 | (0,1,2) |
| 5 | `.churras[draggable="true"]` | 0 | 2 | 0 | (0,2,0) |
| 6 | `#lip1,#lip2,#lip3` | 1 | 0 | 0 | (1,0,0) cada um |
| 7 | `#lip3.fruta` | 1 | 1 | 0 | (1,1,0) |
| 8 | `ol>li.churras#lip5` | 1 | 1 | 2 | (1,1,2) |

**Quem ganha em cada item:**

| Item | Elemento | Regras que combinam | Vencedora | Cor |
|---|---|---|---|---|
| 1 Abacaxi | `li#lip0` | 1 | 1, a única (nenhuma regra usa `#lip0`) | vermelho |
| 2 Melão | `li.fruta` | 1, 3 | 3: (0,1,0) > (0,0,1) | cinza |
| 3 Pera | `li#lip2.fruta` | 1, 3, 6 | 6: tem um id | azul |
| 4 Uva | `li#lip3.fruta` | 1, 3, 6, 7 | 7: (1,1,0) > (1,0,0) | laranja |
| 5 Maçã | `li#lip4.fruta` | 1, 3 | 3 (nenhuma regra usa `#lip4`) | cinza |
| 6 Carne | `li.churras`, logo depois de um `li` | 1, 2, 4 | 4: (0,1,2) > (0,1,0) | preto |
| 7 Linguiça | igual ao item 6 | 1, 2, 4 | 4 | preto |
| 8 Pão de alho | `li#lip7.churras[draggable="true"]` | 1, 2, 4, 5 | 5: (0,**2**,0) > (0,**1**,2) | magenta |

Pegadinhas desse exercício:

- **Ter um id não basta.** Os itens 1 (`lip0`) e 5 (`lip4`) têm id, mas nenhuma regra usa esses ids.
- **A regra 8 parece a mais forte, mas não se aplica a ninguém**, porque nenhum elemento tem `id="lip5"`. A pontuação só importa quando o seletor **combina** com o elemento.
- **Item 8:** a regra 4 tem mais "pedaços" (3), mas a regra 5 tem mais classes/atributos (2 contra 1). Como a coluna B é comparada antes da C, a regra 5 ganha.
- **Item 6:** a Carne combina com `li + li.churras` porque vem logo depois de outro `li` (a Maçã). Os comentários `<!-- -->` entre eles não contam.

### 2.5 Os estilos que o navegador já traz

Mesmo sem CSS nenhum, o navegador aplica estilos próprios: o `h1` é grande e em negrito, o link é azul e sublinhado, o `p` tem margem em cima e embaixo, o `ul` tem bolinhas e recuo, e o `body` tem uma margem de 8px.

🧩 **Na sua atividade:**

- Atividade 10: o `body { margin: 0; }` tira a margem padrão do `body`, para o céu e o mar encostarem nas bordas.
- Atividade 9: o email estava numa `<div>` e os outros dados em `<p>`. O espaçamento ficava diferente porque o `<p>` tem margem padrão e a `<div>` não.

---

## 3. Texto, cores e herança

### 3.1 Formas de escrever cores

| Forma | Exemplo | Como funciona |
|---|---|---|
| Nome | `red`, `skyblue`, `white` | cerca de 140 nomes em inglês |
| Hexadecimal | `#3d4450` | `#RRGGBB`: vermelho, verde e azul, de `00` (nada) a `ff` (máximo) |
| Hex curto | `#fff` | atalho para `#ffffff` |
| `rgb()` | `rgb(221, 21, 144)` | vermelho, verde e azul, de 0 a 255 (atividade 2) |
| `rgba()` | `rgba(0, 0, 0, 0.5)` | igual ao `rgb`, com transparência no final (0 = invisível, 1 = sólido) |
| `transparent` | `transparent` | totalmente transparente |

💡 **Como "ler" um hexadecimal:** números baixos (perto de `00`) = escuro; números altos (perto de `ff`) = claro.

- `#000000` = preto; `#ffffff` = branco.
- `#3d4450` = `3d`, `44`, `50`, todos baixos → **cinza-azulado bem escuro**, quase preto. Por isso, na atividade 9, parecia que o `h1` "não tinha mudado de cor".

💡 **Para testar se uma regra de cor funciona, use uma cor chamativa** (`red`, `lime`). Se mudar, a regra está certa; aí você troca pela cor definitiva.

| Propriedade | Muda |
|---|---|
| `color` | a cor do **texto** |
| `background-color` | a cor do **fundo** da caixa |

### 3.2 Propriedades de texto (atividade 2)

| Propriedade | O que faz | Exemplo |
|---|---|---|
| `color` | cor do texto | `color: red;` |
| `font-size` | tamanho da letra | `60px`, ou palavras como `small` e `large` (atividade 9) |
| `font-family` | tipo de letra | `Arial, Helvetica, sans-serif` |
| `font-weight` | espessura | `bold` (negrito), `normal` |
| `font-style` | estilo | `italic` |
| `text-align` | alinhamento do texto dentro da caixa | `center`, `left`, `right` (atividade 2, no `h1`) |
| `text-shadow` | sombra das letras | `2px 2px 5px yellow` (seção 10.3) |

**O `font-family` é uma lista de reserva:**

```css
font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
```

O navegador tenta a primeira fonte. Se o computador não tiver essa fonte, tenta a segunda, e assim por diante. A última deve ser sempre uma **família genérica**, que existe em qualquer computador:

- `serif`: com "pezinhos" nas letras, como a Times;
- `sans-serif`: sem pezinhos, como a Arial;
- `monospace`: todas as letras com a mesma largura, como a Courier New.

Nomes de fonte com espaço vão entre aspas: `'Courier New'`.

⚠️ `text-align: center` centraliza o **texto** (e os elementos `inline`) dentro da caixa, mas **não** a caixa. Para centralizar uma caixa, use `margin: 0 auto` (seção 4.3) ou flex (seção 5.5).

### 3.3 `<span>`: estilizar só um pedaço do texto

A atividade 2 pede para marcar palavras com `<span>`. O `span` não muda nada sozinho: ele só "embrulha" um pedaço do texto para você poder estilizar esse pedaço.

```html
<p id="p1">Lorem <span class="destaque">ipsum</span> dolor sit amet.</p>
```

```css
.destaque {
    color: red;
    font-weight: bold;
}
```

Só a palavra "ipsum" fica vermelha e em negrito. O resto continua com o estilo do `#p1`.

🔗 **Conexão:** o `span` é um elemento **`inline`** (seção 5.2): fica no meio do texto, sem quebrar a linha, e por isso ignora `width` e `height`.

### 3.4 Herança

Algumas propriedades **passam do pai para os filhos** automaticamente. Outras, não.

| Herdam (propriedades de texto) | Não herdam (propriedades de caixa) |
|---|---|
| `color` | `width`, `height` |
| `font-family`, `font-size`, `font-weight` | `margin`, `padding`, `border` |
| `text-align`, `line-height` | `background-color`, `background-image` |
| | `position`, `display` |

Faz sentido: se o pai tem uma borda, não seria bom que cada filho ganhasse uma borda também.

💡 O fundo **não** é herdado, mas parece que é: o fundo dos filhos é transparente por padrão, então o fundo do pai aparece através deles.

🧩 **Na sua atividade (9):**

```css
#box1 { color: white; }     /* tudo dentro do box1 fica branco */
#box2 { color: #3d4450; }   /* tudo dentro do box2 fica cinza-escuro */
```

Você não precisou escrever a cor no `h1`, no `h3`, no `p` e no `li`: todos herdaram a cor do box.

⚠️ **Uma regra direta no elemento sempre vence a herança**, mesmo que o pai use um seletor forte, como um id:

```css
#box2 { color: gray; }   /* o pai pede cinza */
h1    { color: black; }  /* regra direta no h1 */
```

O `h1` fica **preto**. A herança só é usada quando nenhuma regra fala diretamente com o elemento. É por isso que os links (`a`) não herdam a cor do pai: o próprio navegador já tem uma regra que pinta os links de azul.

---

## 4. Tamanhos, unidades e o modelo de caixa

### 4.1 Unidades de medida

| Unidade | O que é | Na sua atividade |
|---|---|---|
| `px` | pixels: tamanho fixo | quase tudo |
| `%` | porcentagem **de alguma outra coisa** (ver a tabela abaixo) | `width: 25%` (6b), `height: 33.33%` (8), `left: 100%` (10) |
| `vh` / `vw` | 1% da altura / da largura da **janela** do navegador | `height: 100vh` no `body` (10) |
| `deg` | graus, para girar | `rotate(5deg)` (10) |
| `s` / `ms` | segundos / milissegundos, para animações | `2s`, `10s` (10) |
| `em` / `rem` | relativas ao tamanho da fonte | (bom saber) |

O valor `0` não precisa de unidade: `margin: 0` é igual a `margin: 0px`.

**`%` de quê?** Essa é a dúvida mais comum:

| Onde está o `%` | É porcentagem de... |
|---|---|
| `width` | largura do **pai** |
| `height` | altura do **pai**, e só funciona se o pai tiver altura definida |
| `left`, `right` | largura do elemento de referência (seção 6) |
| `top`, `bottom` | altura do elemento de referência |
| `border-radius` | tamanho do **próprio** elemento |
| `translate()` | tamanho do **próprio** elemento |

⚠️ **`height: 100%` só funciona se o pai tiver altura.** Na atividade 9, quando os dois boxes estavam com `position: absolute`, o container ficou com altura 0, e o `height: 100%` do `#box1` virou 100% de 0, ou seja, 0 (seção 12).

### 4.2 Largura e altura padrão

- Um elemento **`block`** (como a `div`) ocupa **toda a largura** do pai, mas a **altura** é só a do conteúdo.
- Uma `div` **vazia** e sem `height` tem altura **0**: ela existe, mas não aparece.

🧩 **Na sua atividade (10):** a `div.barco` não tem nada dentro, então precisa de `width` e `height`. A imagem de fundo **não dá tamanho** à caixa: sem tamanho, a caixa tem altura 0 e a imagem não aparece.

| Propriedade | Significado |
|---|---|
| `height: 600px` | altura **fixa**. Se o conteúdo for maior, ele "vaza" para fora da caixa. |
| `min-height: 600px` | **pelo menos** 600px. Se o conteúdo for maior, a caixa cresce (atividade 9). |
| `max-width: 900px` | **no máximo** 900px (útil em telas grandes). |

**`overflow`: o que fazer com o que passa da caixa**

| Valor | Resultado |
|---|---|
| `visible` | padrão: o conteúdo aparece para fora da caixa |
| `hidden` | o que passa da caixa fica escondido (cortado) |
| `scroll` / `auto` | aparece uma barra de rolagem (`auto` = só quando precisar) |

🧩 **Na sua atividade:**

- Atividade 10: `overflow: hidden` no `body` esconde a barra de rolagem que apareceria quando o barco e a nuvem saem pela direita.
- Atividade 8: `overflow: hidden` no círculo do Brasil corta a faixa branca no formato redondo do círculo (a faixa tem 120px e o círculo só 100px).

### 4.3 O modelo de caixa (box model)

**Todo elemento é uma caixa**, formada por 4 camadas, de dentro para fora:

```
┌─────────────── margin ───────────────┐
│ ┌───────────── border ─────────────┐ │
│ │ ┌────────── padding ───────────┐ │ │
│ │ │                              │ │ │
│ │ │  conteúdo (width × height)   │ │ │
│ │ │                              │ │ │
│ │ └──────────────────────────────┘ │ │
│ └──────────────────────────────────┘ │
└──────────────────────────────────────┘
```

| Camada | O que é | Cor | Exemplo |
|---|---|---|---|
| conteúdo | onde ficam o texto e os filhos | a do fundo | `width: 200px; height: 100px;` |
| `padding` | espaço **interno**, entre o conteúdo e a borda | a do fundo | `padding: 10px;` |
| `border` | a linha em volta | a da borda | `border: 1px solid gray;` |
| `margin` | espaço **externo**, entre a caixa e os vizinhos | transparente | `margin: 20px;` |

O `border` em uma linha leva espessura, estilo e cor, em qualquer ordem: `border: 1px solid gray;` (atividade 7) é igual a `border: solid 1px gray;` (atividade 6c). Estilos possíveis: `solid` (contínua), `dashed` (tracejada), `dotted` (pontilhada) e `none` (sem borda).

**Atalhos com 1, 2 ou 4 valores** (valem para `margin` e `padding`):

| Escrita | Significado |
|---|---|
| `padding: 10px;` | 10px nos 4 lados |
| `padding: 10px 20px;` | 10px em cima e embaixo, 20px nos lados |
| `padding: 10px 20px 30px 40px;` | cima, direita, baixo, esquerda |

💡 Com 4 valores, a ordem é a dos **ponteiros do relógio**, começando ao meio-dia: cima → direita → baixo → esquerda.

💡 `margin: 0 auto;` centraliza na horizontal um elemento `block` que tenha `width`. O `auto` divide igualmente, entre a esquerda e a direita, o espaço que sobra.

### 4.4 `box-sizing`: o `width` inclui o padding e a borda?

| | `content-box` (padrão) | `border-box` |
|---|---|---|
| O `width` vale para | só o conteúdo | conteúdo + padding + borda |
| Padding e borda | são **somados** por fora | ficam **dentro** do `width` |

Exemplo com `width: 200px; padding: 20px; border: 5px solid black;`:

```
content-box:  5 + 20 + 200 + 20 + 5 = 250px na tela
border-box:   200px na tela (o conteúdo encolhe para 150px)
```

Muita gente coloca isto no começo do CSS, para o tamanho que você escreve ser o tamanho que aparece:

```css
* {
    box-sizing: border-box;
}
```

💡 **Truque para enxergar as caixas:** na atividade 9 você usou `* { border: 1px solid gray; }`. Funciona, mas a borda **soma 2px** em cada caixa e pode mexer no layout. O `outline` desenha uma linha parecida que **não ocupa espaço**:

```css
* { outline: 1px solid red; }   /* só para depurar; apague depois */
```

---

## 5. `display` e flexbox

### 5.1 O que o `display` define

O `display` responde a duas perguntas:

- **Por fora:** como o elemento se comporta com os **vizinhos** (fica sozinho na linha ou divide a linha?).
- **Por dentro:** como ele organiza os **filhos** (normal, `flex` ou `grid`).

### 5.2 Valores principais

| Valor | Comportamento | Padrão de |
|---|---|---|
| `block` | ocupa a **linha inteira**, e o próximo elemento vai para baixo. Aceita `width`, `height`, `margin` e `padding`. | `div`, `p`, `h1` a `h6`, `ul`, `section`, `nav` |
| `inline` | fica **no meio do texto**, ao lado dos vizinhos, com a largura do conteúdo. **Ignora** `width` e `height`, e a margem de cima e de baixo não empurra nada. | `span`, `a`, `strong`, `em` |
| `inline-block` | fica lado a lado como o `inline`, mas **aceita** tamanho como o `block`. | `button`, `input` |
| `none` | o elemento **some** e não ocupa espaço, como se não existisse. | — |
| `flex` | por dentro, organiza os filhos em linha ou coluna (seção 5.4). Por fora, é `block`. | — |
| `grid` | por dentro, organiza os filhos em linhas e colunas ao mesmo tempo (seção 5.8). Por fora, é `block`. | — |

```
block:          ┌──────────── div 1 ─────────────┐
                └────────────────────────────────┘
                ┌──────────── div 2 ─────────────┐
                └────────────────────────────────┘

inline:         Texto com [span 1] e [span 2] no meio da linha.

inline-block:   [ caixa 1 ] [ caixa 2 ] [ caixa 3 ]   ← lado a lado, com tamanho
```

🧩 **Na sua atividade (6a):** as quatro boxes são `div`, ou seja, `block`. Mesmo com `width: 300px` (menos que a tela), cada uma fica **sozinha na linha**, uma embaixo da outra. O `block` reserva a linha inteira mesmo quando é estreito.

🧩 **Na sua atividade (6b):** com `display: flex` no `.container`, as mesmas quatro boxes ficam **lado a lado**. Cada uma tem `width: 25%`, então as quatro juntas ocupam 100% da largura.

⚠️ A `img` é `inline` por padrão (mas aceita `width` e `height`). Para centralizar uma imagem com `margin: 0 auto`, ela precisa virar `block`:

```css
img {
    display: block;
    margin: 0 auto;
}
```

**`display: none` × `visibility: hidden`**

- `display: none`: some **e libera o espaço** (os vizinhos ocupam o lugar).
- `visibility: hidden`: fica invisível, mas **o espaço continua reservado** (fica um "buraco").

**Valores menos comuns (bom saber que existem)**

| Valor | O que faz |
|---|---|
| `inline-flex` / `inline-grid` | por dentro, igual a `flex` / `grid`; por fora, divide a linha com os vizinhos |
| `list-item` | faz o elemento se comportar como um `li`, com marcador (é o padrão do `li`) |
| `flow-root` | um `block` que "segura" filhos com `float` (seção 7) |
| `contents` | a caixa do elemento some, mas os filhos continuam, como se estivessem direto no avô |
| `table`, `table-cell`... | imitam partes de uma tabela; hoje o `grid` resolve melhor |

### 5.3 Propriedades do elemento × propriedades que organizam os filhos

Existem **dois tipos** de propriedades:

- **Tipo 1: falam do próprio elemento.** Mudam só o elemento onde você escreve: `width`, `height`, `background-color`, `color`, `font-size`, `border`, `padding`, `margin`.
- **Tipo 2: o pai usa para organizar os filhos.** Você escreve no **pai**, mas o efeito aparece na **posição dos filhos**: `display: flex`, `justify-content`, `align-items`, `flex-direction`, `gap`.

Comparação com uma estante de livros:

- A **estante** (pai) decide: "os livros ficam em pé, um ao lado do outro, centralizados". São as regras de organização (**tipo 2**).
- Cada **livro** (filho) tem o próprio tamanho e a própria cor (**tipo 1**).

A estante não define o tamanho dos livros; ela só os arruma.

🧩 **Na sua atividade (6b):**

```css
.container {
    display: flex;              /* tipo 2: arruma as boxes lado a lado */
}

.box {
    display: flex;              /* tipo 2: arruma o filho DELA (o número) */
    justify-content: center;    /* tipo 2: centraliza o número na horizontal */
    align-items: center;        /* tipo 2: centraliza o número na vertical */
    height: 150px;              /* tipo 1: tamanho da própria box */
    width: 25%;                 /* tipo 1: tamanho da própria box */
}
```

O `.box` é ao mesmo tempo **filho** do container e **pai** do número. Por isso tem os dois tipos de propriedade.

💡 **Pergunta para testar:** "isso muda o elemento ou a posição do que está dentro dele?" Se muda o elemento, é tipo 1. Se muda a posição do que está dentro, é tipo 2, e precisa de `display: flex` (ou `grid`).

### 5.4 Flexbox: os dois eixos

Com `display: flex`, o elemento vira um **container flex**, e cada **filho direto** vira um **item flex**.

O flex trabalha com dois eixos:

```
flex-direction: row (padrão)

eixo principal ──────────────────────→   (justify-content)

┌──────────────────────────────────────┐
│ ┌──────┐ ┌──────┐ ┌──────┐           │   eixo cruzado
│ │item 1│ │item 2│ │item 3│           │   ↓ (align-items)
│ └──────┘ └──────┘ └──────┘           │
└──────────────────────────────────────┘
```

Com `flex-direction: column`, os eixos **trocam**: o principal fica vertical e o cruzado fica horizontal. Aí o `justify-content` passa a controlar a vertical, e o `align-items`, a horizontal.

**Propriedades do pai (container):**

| Propriedade | Valores comuns | O que faz |
|---|---|---|
| `flex-direction` | `row` (padrão), `column` | filhos em linha ou em coluna |
| `justify-content` | `flex-start`, `center`, `flex-end`, `space-between`, `space-around`, `space-evenly` | distribui os filhos no eixo **principal** |
| `align-items` | `stretch` (padrão), `flex-start`, `center`, `flex-end` | alinha os filhos no eixo **cruzado** |
| `gap` | `10px` | espaço entre os filhos |
| `flex-wrap` | `nowrap` (padrão), `wrap` | `wrap` deixa os filhos quebrarem para a linha de baixo quando não cabem |
| `align-content` | os mesmos do `justify-content` | alinha as **linhas** entre si, quando há mais de uma (com `wrap`) |

**Os valores do `justify-content`**, com 3 itens numa linha:

```
flex-start     │[1][2][3]                     │
center         │          [1][2][3]           │
flex-end       │                     [1][2][3]│
space-between  │[1]          [2]           [3]│
space-around   │   [1]       [2]       [3]    │
space-evenly   │     [1]     [2]     [3]      │
```

- `space-between`: o primeiro encosta no início, o último no fim, e o espaço que sobra fica **entre** eles.
- `space-around`: cada item ganha o mesmo espaço dos dois lados (por isso as pontas ficam com metade).
- `space-evenly`: todos os espaços são iguais, inclusive nas pontas.

🧩 **Na sua atividade (7):** `flex-direction: column` + `justify-content: space-between` no chat. As mensagens ficam distribuídas de cima a baixo: a primeira no topo e a caixa de digitar encostada embaixo.

💡 **O `align-items: stretch` é o padrão.** Se os itens não tiverem altura definida, eles **esticam** até a altura do container.

🧩 **Na sua atividade (9):** por causa do `stretch`, a coluna escura (`#box1`) e o conteúdo (`#box2`) ficam sempre com a **mesma altura**, sem você escrever `height` neles.

### 5.5 Centralizando com flex

```css
.box {
    display: flex;
    justify-content: center;   /* meio na horizontal ↔ */
    align-items: center;       /* meio na vertical ↕ */
}
```

Você usou isso nas boxes das atividades 6 e 7, para o número ou o texto ficar no meio da caixa.

⚠️ `justify-content` e `align-items` só funcionam com `display: flex` (ou `grid`). Se você apagar a linha do `display`, as outras duas param de ter efeito, mesmo continuando no código.

### 5.6 Propriedades que vão no filho (item flex)

São escritas no **filho**, mas só funcionam se o **pai** tiver `display: flex`:

| Propriedade | O que faz |
|---|---|
| `flex: 1` | o filho **cresce** para ocupar o espaço que sobra |
| `flex-grow` | quanto o filho cresce (`0` = não cresce) |
| `flex-shrink` | quanto o filho encolhe quando falta espaço (`0` = não encolhe) |
| `flex-basis` | tamanho inicial, antes de crescer ou encolher |
| `align-self` | alinha **só esse filho**, ignorando o `align-items` do pai |
| `order` | muda a ordem na tela sem mexer no HTML (o menor número vem primeiro) |

**O `flex: 1` em detalhe (atividade 9):**

```
.container = 900px
├── #box1 = width: 250px   (tamanho fixo)
└── #box2 = flex: 1        → fica com o resto: 900 − 250 = 650px
```

Sem o `flex: 1`, o `#box2` teria só a largura do próprio conteúdo. Com ele, o `#box2` se ajusta sozinho se você mudar a largura do container.

O número é uma **proporção** do espaço que sobra:

- dois filhos com `flex: 1` → metade para cada um;
- um filho com `flex: 2` e outro com `flex: 1` → o primeiro fica com 2/3 e o segundo com 1/3.

**O `align-self` (exemplo com o chat):**

```css
.container { display: flex; flex-direction: column; }
.eu        { align-self: flex-end; }   /* só esse filho vai para o fim (a direita, porque é coluna) */
```

### 5.7 ⚠️ No flex, todo filho direto vira um item

Este foi o "bug" da atividade 9. O texto da lateral ficou **fora** do `#box1`, porque o `</div>` fechava logo depois de "Pessoal":

```html
<div class="container">                       <!-- display: flex -->
    <div class="box" id="box1">Pessoal</div>  <!-- fechou cedo demais! -->
    <div class="nome">Nome</div>              <!-- virou filho do container -->
    <p>Joana Barbosa</p>                      <!-- virou filho do container -->
    <div class="box" id="box2">box2</div>
</div>
```

O container passou a ter **4 filhos**, e o flex colocou os quatro lado a lado, como 4 colunas:

```
Errado:  [ box1: Pessoal ][ Nome ][ Joana Barbosa ][ box2 ]
Certo:   [ box1: Pessoal, Nome, Joana Barbosa...  ][ box2 ]
```

A correção foi fechar o `#box1` só **depois** de todo o conteúdo da lateral.

💡 Quando um layout com flex "explode" em colunas inesperadas, a primeira coisa a conferir é **onde cada `</div>` fecha**.

### 5.8 Bom saber: grid

O `grid` organiza os filhos em **linhas e colunas ao mesmo tempo**. Ele apareceu no começo da sua atividade 9 (`display: grid; grid-template-columns: 50px 1fr 2fr;`). O layout do perfil também poderia ser feito assim:

```css
.container {
    display: grid;
    grid-template-columns: 250px 1fr;   /* coluna 1 fixa, coluna 2 com o resto */
}
```

A unidade `fr` ("fração") divide o espaço que sobra, parecido com o `flex: 1`.

---

## 6. `position` e `z-index`

### 6.1 Primeiro: o que é o "fluxo normal"

Sem `position`, o navegador coloca os elementos **um depois do outro, na ordem do HTML**: os `block` empilhados e os `inline` lado a lado no texto. Isso é o **fluxo normal**.

Comparação: o fluxo é uma **fila**. Cada elemento ocupa um lugar, e o próximo fica logo depois.

- O `display` decide **como** cada um ocupa o seu lugar na fila.
- O `position` decide se o elemento **fica na fila** ou **sai dela** para ficar num lugar exato.

### 6.2 Os valores de `position`

| Valor | Sai do fluxo? | `top`, `left`... são medidos a partir de | Característica |
|---|---|---|---|
| `static` | não | — (`top`, `left` etc. **não funcionam**) | é o **padrão** de todo elemento |
| `relative` | não | da **posição original** do próprio elemento | continua ocupando o lugar original. Muito usado **no pai**, para ele virar a referência dos filhos `absolute` |
| `absolute` | **sim** | do **ancestral posicionado mais próximo** (qualquer `position` diferente de `static`). Se não houver nenhum, da própria janela, no topo da página | não ocupa espaço: os vizinhos agem como se ele não existisse |
| `fixed` | **sim** | da **janela** do navegador | fica parado na tela mesmo quando a página rola (menus fixos, botão "voltar ao topo") |
| `sticky` | não | do container que rola | se comporta normalmente até a rolagem chegar no valor do `top`, e aí "gruda" ali (cabeçalhos que acompanham a rolagem) |

> ⚠️ `top`, `right`, `bottom` e `left` só funcionam quando o `position` **não é** `static`.

**O que acontece quando um elemento sai da fila (`absolute`):**

```
Fluxo normal:        B com position: absolute:

[ A ]                [ A ]
[ B ]                [ C ]  ← C sobe e ocupa o lugar de B
[ C ]                     [ B ]  ← B fica onde o top/left mandar
                                   (pode ficar por cima dos outros)
```

Consequências:

1. Os vizinhos ocupam o lugar dele.
2. **O pai não "enxerga" o filho `absolute`**: ele não conta para a altura do pai. Esse foi o problema da atividade 9 (seção 12).
3. Sem `width`, ele fica só do tamanho do conteúdo.
4. Sem `top`, `left` etc., ele fica onde estaria no fluxo, mas já sem ocupar espaço.

### 6.3 A dupla mais usada: pai `relative` + filho `absolute`

```css
.pai {
    position: relative;   /* vira a referência, mas continua no lugar dele */
}

.filho {
    position: absolute;   /* se posiciona em relação ao .pai */
    top: 0;
    right: 0;             /* canto de cima à direita DO PAI */
}
```

Sem o `position: relative` no pai, o `absolute` procura um ancestral posicionado, não encontra nenhum e usa a página como referência.

Pense em `top`, `right`, `bottom` e `left` como a **distância até a borda** correspondente do pai:

```
     pai com position: relative
┌──────────────────────────────┐
│[1]                        [2]│   1 → top: 0;    left: 0;
│                              │   2 → top: 0;    right: 0;
│             [5]              │   3 → bottom: 0; right: 0;
│                              │   4 → bottom: 0; left: 0;
│[4]                        [3]│   5 → centro (seção 6.4)
└──────────────────────────────┘
```

🧩 **Na sua atividade:**

- **6c:** quatro boxes nos cantos e uma no centro de um container `relative`.
- **7:** o botão "Enviar" no canto da caixa de mensagem:

  ```css
  .input        { position: relative; }
  .input button { position: absolute; bottom: 8px; right: 8px; }
  ```

- **8:** a `.bandeira` é `relative`, e os círculos, o losango e a faixa são `absolute` dentro dela.
- **10:** o barco, o mar e a nuvem são `absolute`. Como nenhum pai deles tem `position`, a referência é a própria janela: `bottom: 0` = colado embaixo da tela.

### 6.4 Duas formas de centralizar com `position`

**Forma 1: fazer a conta (atividade 6c)**

```
(tamanho do container − tamanho da box) ÷ 2
(600 − 150) ÷ 2 = 225px   →   top: 225px; left: 225px;
```

Funciona, mas, se você mudar algum tamanho, precisa refazer a conta.

**Forma 2: `50%` + `translate(-50%, -50%)` (atividade 8)**

```css
.circulo {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

Por que funciona:

**Passo 1:** `top: 50%; left: 50%` coloca o **canto de cima à esquerda** do círculo no centro do pai. O círculo fica deslocado para baixo e para a direita:

```
┌─────────────────┐
│                 │
│                 │
│        ┌───┐    │
│        │   │    │
│        └───┘    │
└─────────────────┘
```

**Passo 2:** `translate(-50%, -50%)` puxa o círculo de volta **metade do tamanho dele** para cima e para a esquerda. Aqui o `%` é do **próprio** elemento (seção 4.1). Agora o centro do círculo está no centro do pai:

```
┌─────────────────┐
│                 │
│      ┌───┐      │
│      │ ● │      │
│      └───┘      │
│                 │
└─────────────────┘
```

A vantagem é que não precisa de conta: funciona com qualquer tamanho.

### 6.5 `position` × `display`

| | `display` | `position` |
|---|---|---|
| Pergunta que responde | "como o elemento ocupa espaço e organiza os filhos?" | "o elemento fica onde o fluxo mandar ou num lugar exato?" |
| Quem decide o lugar | o **pai** organiza os filhos (flex, grid...) | o **próprio elemento** diz onde fica (`top`, `left`...) |
| Bom para | grupos: linhas, colunas, distribuir espaço | lugares exatos: cantos, centro, um elemento por cima de outro |

Comparação com a estante: o `display` é a estante arrumando os livros; o `position` é tirar um livro da estante e colocá-lo num lugar exato, como "em cima da mesa, no canto esquerdo".

Como um interfere no outro:

1. **`absolute` e `fixed` tiram o elemento do fluxo.** Num container flex, um filho `absolute` **ignora** o flex, e os outros filhos se arrumam como se ele não existisse.
2. **`absolute` e `fixed` fazem o elemento virar `block` por fora.** Um `span` com `position: absolute` passa a aceitar `width` e `height`.
3. **`display: none` vence tudo.** O elemento some, qualquer que seja o `position`.
4. **Dá para usar os dois juntos.** Na atividade 6c, cada box é `absolute` (para ir ao canto) e tem `display: flex` (para centralizar o texto dentro dela).

Quando usar cada um:

- vários elementos em linha, coluna ou grade → `display` (flex ou grid);
- um elemento num lugar exato (canto, centro, por cima de outro) → `position`;
- um elemento que acompanha a rolagem → `fixed` ou `sticky`.

### 6.6 `z-index`: quem fica na frente

Quando elementos se sobrepõem, o `z-index` decide **quem fica na frente de quem**. Pense numa pilha de papéis:

- número **maior** = mais na **frente**;
- número **menor** = mais **atrás** (pode ser negativo).

**Sem `z-index`:** quem vem **depois** no HTML fica na frente. E um elemento com `position` (`relative`, `absolute`...) fica na frente dos que não têm.

⚠️ O `z-index` só funciona em elementos com `position` diferente de `static` (ou em filhos de um container flex ou grid).

🧩 **Na sua atividade (10):**

```
z-index: 2   →  barco   (na frente)
z-index: 1   →  mar     (atrás do barco)
(sem)        →  céu, que é o fundo do body (atrás de tudo)
```

O barco fica um pouco "dentro" da água. Sem os `z-index`, o mar cobriria o barco, porque a `div.mar` vem **depois** da `div.barco` no HTML.

💡 As ondas (`::before` e `::after`) ficam **dentro** do `.mar`, então fazem parte da "camada" dele. Por isso o barco também fica na frente delas.

---

## 7. `float` e `clear`

### 7.1 O que o `float` faz

O `float` empurra o elemento para um dos lados e deixa **o conteúdo seguinte contornar ele**, como uma foto no canto de uma matéria de jornal, com o texto em volta.

| Valor | Significado |
|---|---|
| `left` / `inline-start` | flutua para a esquerda (início da linha) |
| `right` / `inline-end` | flutua para a direita (fim da linha) |
| `none` | padrão: não flutua |

💡 `inline-start` e `inline-end` são as versões "modernas" de `left` e `right`. Em português, que se escreve da esquerda para a direita, dá no mesmo.

O elemento com `float` sai **parcialmente** do fluxo:

- os elementos `block` que vêm depois **sobem** e ocupam o lugar dele;
- mas o **texto** desses elementos desvia do float e fica ao lado dele.

```
┌──────┐ Este texto contorna a imagem
│ img  │ flutuada. Quando a imagem
│float │ acaba, o texto volta a ocupar
└──────┘ a linha inteira, como aqui
embaixo, sem nenhum espaço à esquerda.
```

### 7.2 O que o `clear` faz

O `clear` vai no elemento que vem **depois** do float e diz: "não fique ao lado de nenhum float; comece abaixo dele".

| Valor | Significado |
|---|---|
| `left` | desce abaixo dos floats da esquerda |
| `right` | desce abaixo dos floats da direita |
| `both` | desce abaixo de qualquer float, dos dois lados (o mais usado) |
| `none` | padrão: pode ficar ao lado dos floats |

Comparação com o jornal: o `float` é a foto encostada no canto; o `clear: both` é um parágrafo que diz "eu só começo depois que a foto acabar".

### 7.3 ⚠️ Dois cuidados

**1. O `float` não funciona em filho de flex.** Se o pai tem `display: flex` (ou `grid`), o `float` dos filhos é **ignorado**. A sintaxe está certa, mas não tem efeito nenhum. A solução é colocar o elemento dentro de uma `div` comum: essa `div` vira o item do flex, e dentro dela o `float` volta a funcionar.

**2. O pai "não enxerga" o float.** Se uma `div` só tem filhos com `float`, a altura dela fica **zero**, porque o float não conta para o tamanho do pai (como o `absolute`, seção 6.2). Formas de resolver:

- colocar, depois do float, um elemento com `clear: both` (o caso da atividade 7);
- ou usar `display: flow-root` no pai.

### 7.4 🧩 Na sua atividade (7, chat)

```html
<div class="container">          <!-- display: flex -->
    <div class="msg">            <!-- filho do flex: uma div comum -->
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
    float: inline-start;        /* funciona porque o pai é a .msg, e não o container */
}

.box {
    clear: both;                /* o balão começa ABAIXO do nome, e não ao lado */
}
```

| Situação | Resultado na tela |
|---|---|
| `.nome` direto dentro do `.container` (sem a `.msg`) | o `float` é ignorado, porque o pai é flex |
| com a `.msg`, mas sem `clear` no `.box` | o nome fica à esquerda e o balão **ao lado** dele |
| com a `.msg` e `clear: both` no `.box` | o nome fica em cima e o balão **embaixo** ✅ |

### 7.5 Quando usar `float` hoje?

Hoje o `float` é usado principalmente para **texto contornando uma imagem**. Para montar layout (colunas, alinhar à esquerda ou à direita), `flex` e `grid` resolvem melhor. Mesmo assim, vale entender o `float`, porque ele aparece em código antigo e em exercícios.

---

## 8. Fundos, gradientes e formas

### 8.1 Imagem de fundo (atividade 10)

```css
.barco {
    width: 100px;                            /* obrigatório numa div vazia */
    height: 100px;                           /* obrigatório numa div vazia */
    background-image: url("barquinho.jpg");  /* caminho da imagem */
    background-size: contain;                /* a imagem inteira dentro da caixa */
    background-repeat: no-repeat;            /* sem repetir */
}
```

| Propriedade | Valores | O que faz |
|---|---|---|
| `background-image` | `url("arquivo.png")` | a imagem. O caminho parte do arquivo HTML: `"barquinho.jpg"` = na mesma pasta. Num arquivo `.css` externo, o caminho parte do arquivo `.css`. |
| `background-size` | `auto` (padrão), `contain`, `cover`, `100px 50px` | `auto` = tamanho original; `contain` = a imagem cabe inteira (pode sobrar espaço); `cover` = cobre a caixa toda (pode cortar) |
| `background-repeat` | `repeat` (padrão), `no-repeat`, `repeat-x`, `repeat-y` | se a imagem se repete como azulejo |
| `background-position` | `center`, `top left`, `0 0` | onde a imagem fica dentro da caixa |

⚠️ **Sem `width` e `height`, a imagem de fundo não aparece**, porque a div vazia tem altura 0 (seção 4.2). O `background-image` decora a caixa, mas não dá tamanho a ela.

⚠️ **Sem `background-size` e `no-repeat`**, a imagem aparece no tamanho original (se for grande, só um pedaço cabe na caixa) e se repete como azulejo (se for pequena).

⚠️ **JPG não tem transparência.** Se a imagem do barco tiver fundo branco, aparece um retângulo branco em volta dele. Para aparecer só o barco, use um **PNG com fundo transparente**.

### 8.2 `linear-gradient`: degradê em linha reta

```css
background-image: linear-gradient(skyblue, white);
```

Começa em `skyblue` em cima e termina em `white` embaixo. É o céu da atividade 10.

⚠️ O gradiente é uma **imagem** gerada pelo navegador, então vai em `background-image` (ou `background`), **nunca** em `background-color`.

| Escrita | Resultado |
|---|---|
| `linear-gradient(skyblue, white)` | de cima para baixo (padrão) |
| `linear-gradient(to right, red, blue)` | da esquerda para a direita |
| `linear-gradient(45deg, red, blue)` | na diagonal |
| `linear-gradient(red, yellow, green)` | três cores, distribuídas igualmente |
| `linear-gradient(red 50%, blue 50%)` | metade vermelha, metade azul, **sem degradê** |

💡 **Corte seco:** quando duas cores usam a mesma porcentagem, não há transição, e a troca de cor é instantânea. Com isso dá para fazer listras.

🔗 **Conexão com a atividade 8:** a bandeira da Alemanha poderia ser feita **sem nenhuma div interna**:

```css
#alemanha.bandeira {
    background-image: linear-gradient(
        #000000 33.33%,           /* preto de 0% a 33.33% */
        #dd0000 33.33% 66.66%,    /* vermelho de 33.33% a 66.66% */
        #ffce00 66.66%            /* amarelo de 66.66% até o fim */
    );
}
```

A atividade pede divs internas com combinação de seletores, então a sua versão é a certa para o exercício. Isto é só para mostrar outra ferramenta.

⚠️ **O `body` precisa de altura para o degradê cobrir a tela.** Na atividade 10, todos os elementos são `absolute`, então nenhum conta para a altura do `body`, e ele ficaria sem altura. O `height: 100vh` dá ao `body` a altura da janela, e o degradê se espalha do topo até embaixo.

### 8.3 `radial-gradient`: degradê em círculo

Sai do centro para fora:

```css
background-image: radial-gradient(white, blue);   /* branco no meio, azul nas bordas */
```

Com corte seco, vira um **círculo** desenhado no fundo:

```css
background-image: radial-gradient(circle, blue 70%, transparent 71%);
```

"Azul até 70%, transparente a partir de 71%" → um círculo azul com a borda nítida.

💡 Foi assim que o professor sugeriu fazer as ondas: com `background-size: 40px 40px` e `background-repeat: repeat-x`, esse círculo se repete numa fileira de bolinhas, e a metade de cima delas fica acima da linha do mar. A sua versão usou `border-radius` (seção 8.4), que o exercício também aceita ("ou outro método criativo").

### 8.4 `border-radius`: cantos arredondados, círculos e ovais

| Valor | Resultado |
|---|---|
| `border-radius: 10px` | cantos levemente arredondados |
| `border-radius: 50%` num **quadrado** | **círculo** (Japão, nuvem) |
| `border-radius: 50%` num **retângulo** | **oval** (as ondas: metade da tela de largura × 40px de altura) |
| `border-radius: 20px 20px 20px 0` | um valor por canto: cima-esquerda, cima-direita, baixo-direita, baixo-esquerda (útil para balão de chat) |

💡 Qualquer valor acima de `50%` dá o mesmo resultado que `50%`: o navegador não deixa arredondar "mais que redondo". O `150%` da sua nuvem funciona, mas é igual a `50%`.

🔗 O `border-radius` também arredonda a **sombra** (`box-shadow`, seção 10) e o **recorte** do `overflow: hidden`. É por isso que o círculo do Brasil corta a faixa branca em forma redonda.

### 8.5 `clip-path`: recortar formas (atividade 8, losango)

O `clip-path` recorta o elemento: só a parte de dentro da forma aparece.

```css
clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);
```

Cada par `x y` é um ponto, medido a partir do canto de cima à esquerda do próprio elemento. O navegador liga os pontos em ordem e fecha a forma:

| Ponto | x | y | Onde fica |
|---|---|---|---|
| `50% 0` | meio da largura | topo | ponta de cima |
| `100% 50%` | borda direita | meio da altura | ponta da direita |
| `50% 100%` | meio da largura | base | ponta de baixo |
| `0 50%` | borda esquerda | meio da altura | ponta da esquerda |

```
┌───────●───────┐
│    /     \    │
│  /         \  │
●               ●
│  \         /  │
│    \     /    │
└───────●───────┘
```

Tudo o que fica fora do losango (os 4 cantos do retângulo) é cortado, e aparece o verde do fundo da bandeira.

**Por que não `rotate(45deg)`?** Girar um quadrado também dá um losango, mas com os 4 lados iguais. O losango do Brasil é mais largo do que alto. Com `clip-path`, a forma acompanha o tamanho da `div` (250 × 166px).

Outras formas:

```css
clip-path: polygon(50% 0, 100% 100%, 0 100%);   /* triângulo */
clip-path: circle(50%);                          /* círculo */
```

### 8.6 🧩 Juntando tudo: a bandeira do Brasil (atividade 8)

```
#brasil.bandeira       → fundo verde; position: relative (é a referência)
├── div.losango        → absolute + clip-path (losango amarelo)
└── div.circulo        → absolute + centralizado com translate + border-radius: 50%
    └── div.faixa      → absolute + rotate(10deg), cortada pelo overflow: hidden do círculo
```

Só nessa bandeira aparecem: seletores combinados (2.2), pai `relative` + filho `absolute` (6.3), centralização com `translate` (6.4), `border-radius` (8.4), `clip-path` (8.5), `rotate` (11.1) e `overflow: hidden` (4.2).

---

## 9. Pseudo-elementos `::before` e `::after`

### 9.1 O que são

O `::before` e o `::after` criam **dois elementos extras dentro de um elemento, só pelo CSS**, sem escrever nada no HTML.

No seu HTML só existe isto:

```html
<div class="mar"></div>
```

Mas, com o CSS, o navegador monta assim:

```
<div class="mar">
    ::before   ← criado no começo, dentro do .mar
    ::after    ← criado no final, dentro do .mar
</div>
```

Eles são "filhos" do `.mar`. Na atividade 10, você ganhou duas ondas sem criar nenhuma `div` a mais no HTML.

### 9.2 O `content` é obrigatório

```css
content: "";
```

O `content` define **o que vai escrito dentro** do pseudo-elemento:

- com texto entre as aspas, o texto aparece na tela;
- com aspas vazias (`""`), não aparece texto nenhum, mas o elemento é criado.

⚠️ **Sem o `content`, o navegador nem cria o pseudo-elemento.** O tamanho, a cor e a animação continuam escritos, mas não existe nada para aplicar. Isso vale só para `::before` e `::after`; em elementos normais você não usa `content`.

Exemplo com texto:

```css
h2::before {
    content: "→ ";     /* coloca uma seta antes de todo h2 */
}
```

### 9.3 Como posicionar

Por padrão, os pseudo-elementos são `inline` (como um `span`). Para usá-los como desenho:

- dê `position: absolute` a eles (assim eles aceitam `width` e `height`, seção 6.5);
- e garanta que o pai tenha `position` diferente de `static`, para ser a referência.

🧩 **Na sua atividade (10):** o `.mar` já é `position: absolute`, então ele é a referência das ondas. Se o `.mar` fosse `static`, o `top: -20px` das ondas seria medido a partir da janela, e elas iriam parar no topo da página.

💡 `::before` e `::after` não funcionam em `img` e `input`.

### 9.4 🧩 As ondas da atividade 10, passo a passo

```css
.mar::before,
.mar::after {
    content: "";              /* obrigatório */
    position: absolute;       /* posiciona em relação ao .mar */
    top: -20px;               /* sobe 20px: metade da oval fica fora do mar */
    width: 50%;               /* cada onda ocupa metade da largura */
    height: 40px;
    background-color: blue;   /* a mesma cor do mar */
    border-radius: 50%;       /* vira uma oval */
    animation: onda 2s infinite alternate;
}

.mar::after {
    left: 50%;                /* a segunda onda vai para a metade da direita */
}

@keyframes onda {
    from { top: -20px; }
    to   { top: -10px; }
}
```

**Como o efeito de onda acontece:**

1. **Ovais da mesma cor do mar.** As ovais são azuis, iguais ao retângulo do mar.
2. **Metade para fora.** Cada oval tem 40px de altura e `top: -20px`. Metade fica **acima** da borda do mar, e a outra metade, **dentro** dele:

   ```
    /¯¯¯¯¯¯¯¯¯¯¯¯\      ← metade de cima: fica acima do mar e aparece
   ─────────────────    ← borda de cima do mar
    \____________/      ← metade de baixo: fica dentro do mar e some (mesma cor)
   ```

   A metade de baixo se mistura com o mar. Só a de cima aparece, como uma "lombada" na água.
3. **Sobe e desce.** A animação leva as ovais de `top: -20px` a `top: -10px` e volta (`alternate`), sem parar (`infinite`). A lombada cresce e diminui, como a água balançando.

Para mudar o efeito:

- **onda mais alta:** aumente o `height` ou use um `top` mais negativo;
- **mais ondas:** diminua o `width` (com `25%`, por exemplo) e ajuste o `left`;
- **mais lenta ou mais rápida:** mude o `2s`;
- **ondas desencontradas:** coloque `animation-delay: 1s;` só no `::after`.

🔗 **Conexões:** `content` (9.2), referência do `position` (6.3), oval com `border-radius` (8.4), `alternate` (11.4).

---

## 10. Sombras: `box-shadow` e `text-shadow`

### 10.1 `box-shadow`

```
box-shadow: x  y  desfoque  espalhamento  cor;
```

| Parte | O que faz | Obrigatória? |
|---|---|---|
| x | desloca para os lados (positivo = direita, negativo = esquerda) | sim |
| y | desloca para cima ou para baixo (positivo = baixo, negativo = cima) | sim |
| desfoque | borra a sombra; sem ele, a borda fica nítida | não |
| espalhamento | aumenta (ou diminui, se negativo) o tamanho da sombra | não |
| cor | a cor da sombra | recomendada |

Existe também a palavra `inset`, que faz a sombra ficar para dentro da caixa.

**Uso normal (sombra de verdade):**

```css
box-shadow: 5px 5px 10px gray;
```

Sombra cinza, borrada, um pouco para a direita e para baixo: dá a impressão de que a caixa está "levantada" da página.

Características:

- a sombra tem a **mesma forma** do elemento (se o elemento é redondo, a sombra também é);
- a sombra **não ocupa espaço** e não empurra os vizinhos;
- dá para ter **várias sombras**, separadas por vírgula.

### 10.2 🧩 O truque da nuvem (atividade 10)

```css
.nuvem {
    width: 60px;
    height: 60px;
    background-color: white;
    border-radius: 50%;
    box-shadow: 40px 0 white, 20px -25px white;
}
```

Sem desfoque e com a mesma cor da nuvem, cada sombra vira uma **cópia nítida do círculo**:

- `40px 0 white` → uma cópia 40px para a direita;
- `20px -25px white` → uma cópia 20px para a direita e 25px para cima.

```
       ( 3 )         ← 3 = cópia: 20px para a direita e 25px para cima
   ( 1 )   ( 2 )     ← 1 = o círculo original; 2 = cópia 40px para a direita
```

Os 3 círculos juntos formam a nuvem.

🔗 **Conexão com a animação:** a nuvem "de verdade" (a div) tem 60px, mas o desenho, com as cópias, chega a 100px de largura (de 0 até 40 + 60). Por isso a animação dela começa em `left: -120px`: para o desenho inteiro começar escondido fora da tela (seção 11.6).

### 10.3 `text-shadow` (atividade 2)

```
text-shadow: x  y  desfoque  cor;
```

```css
text-shadow: 2px 2px 5px yellow;
```

Sombra amarela, 2px para a direita e para baixo, borrada em 5px. É a mesma ideia do `box-shadow`, mas aplicada às letras (e sem o espalhamento).

---

## 11. Animação: `transform`, `@keyframes` e `animation`

### 11.1 `transform`: mexer no elemento sem mexer no layout

| Função | O que faz | Na sua atividade |
|---|---|---|
| `translate(x, y)` | desloca | `translate(-50%, -50%)` para centralizar (8) |
| `rotate(ângulo)` | gira (positivo = sentido horário) | `rotate(10deg)` na faixa do Brasil (8); `rotate(5deg)` no balanço do barco (10) |
| `scale(n)` | aumenta ou diminui (`1` = normal, `2` = dobro, `0.5` = metade) | (bom saber) |

Características:

- **Não mexe no fluxo:** o espaço original do elemento continua reservado, e os vizinhos não se movem (parecido com `position: relative`).
- Várias funções vão na **mesma linha**, separadas por espaço:

  ```css
  transform: translate(-50%, -50%) rotate(10deg);
  ```

- ⚠️ Escrever `transform` duas vezes **não soma**: a segunda substitui a primeira (cascata, seção 2.3).

**`transform-origin`: o ponto em volta do qual o elemento gira.** O padrão é o centro.

💡 Para o barco, `transform-origin: bottom center;` faz ele balançar a partir da base, como um barco de verdade, em vez de girar pelo meio.

### 11.2 As duas partes de uma animação

1. **`@keyframes`** é o **roteiro**: diz o que muda e em que momento. Fica solto no CSS, fora de qualquer regra.
2. **`animation`** é o **play**: aplica o roteiro a um elemento e diz a duração, quantas vezes repete etc.

Comparação: o `@keyframes` é a coreografia escrita no papel; o `animation` é chamar alguém para dançar essa coreografia, dizendo em quanto tempo e quantas vezes.

Sem o `animation`, o `@keyframes` não faz nada. Sem o `@keyframes`, o `animation` não tem o que executar.

### 11.3 Escrevendo o `@keyframes`

```css
@keyframes balanco {
    0%   { transform: rotate(-5deg); }   /* começo */
    50%  { transform: rotate(5deg); }    /* meio */
    100% { transform: rotate(-5deg); }   /* fim */
}
```

- `balanco` é o **nome**: você escolhe (evite espaços e acentos).
- As **porcentagens** são momentos da animação: 0% = início, 100% = fim.
- `from` é o mesmo que `0%`, e `to` é o mesmo que `100%`.
- Dentro de cada momento vão **chaves** com as propriedades.
- O navegador calcula sozinho todos os quadros intermediários.

⚠️ **Erro que já apareceu (atividade 10):** escrever a propriedade direto dentro do `@keyframes`:

```css
/* ❌ errado: falta dizer EM QUE MOMENTO */
@keyframes balanco {
    transform: rotate(5deg);
}

/* ✅ certo: cada propriedade dentro de um momento */
@keyframes balanco {
    0%   { transform: rotate(-5deg); }
    100% { transform: rotate(5deg); }
}
```

💡 Momentos com o mesmo estilo podem ser juntados com vírgula:

```css
@keyframes balanco {
    0%, 100% { transform: rotate(-5deg); }
    50%      { transform: rotate(5deg); }
}
```

### 11.4 A propriedade `animation`

O atalho, tudo numa linha:

```
animation: nome  duração  ritmo  atraso  repetições  direção;
```

```css
animation: balanco 2s linear 1s infinite;
```

- `balanco` → qual `@keyframes` usar;
- `2s` → duração de **uma** volta;
- `linear` → o ritmo;
- `1s` → atraso antes de começar;
- `infinite` → repete para sempre.

⚠️ Quando há dois tempos, **o primeiro é sempre a duração, e o segundo, o atraso**. `2s 1s` é diferente de `1s 2s`.

| Propriedade separada | Valores | O que faz |
|---|---|---|
| `animation-name` | o nome do `@keyframes` | qual roteiro usar |
| `animation-duration` | `2s`, `500ms` | quanto tempo dura uma volta |
| `animation-timing-function` | `ease` (padrão), `linear`, `ease-in`, `ease-out`, `ease-in-out`, `cubic-bezier(...)` | o ritmo (ver abaixo) |
| `animation-delay` | `1s` | espera antes de começar |
| `animation-iteration-count` | `1` (padrão), `3`, `infinite` | quantas vezes repete |
| `animation-direction` | `normal`, `reverse`, `alternate` | o sentido (ver abaixo) |
| `animation-fill-mode` | `none`, `forwards`, `backwards`, `both` | o que mostrar antes de começar e depois de terminar |

**O ritmo (`timing-function`)**

| Valor | Como fica | Bom para |
|---|---|---|
| `linear` | velocidade **constante** | movimentos que atravessam a tela (barco, nuvem) |
| `ease` (padrão) | começa devagar, acelera e termina devagar | uso geral |
| `ease-in` | começa devagar e acelera | algo saindo de cena |
| `ease-out` | começa rápido e freia | algo chegando |
| `ease-in-out` | devagar nas pontas, rápido no meio | balanços e vai e vem (fica mais natural) |
| `cubic-bezier(...)` | ritmo personalizado | a lua (`#moon`) da sua outra animação usou um |

**A direção (`direction`)**

| Valor | Sequência das voltas |
|---|---|
| `normal` | 0%→100%, 0%→100%... (volta "pulando" para o começo) |
| `reverse` | 100%→0%, 100%→0%... |
| `alternate` | 0%→100%, 100%→0%, 0%→100%... (vai e volta, sem pulo) |

**Duas formas de fazer "vai e volta"** (as duas estão na atividade 10):

| Técnica | Onde | Como |
|---|---|---|
| repetir o começo no 100% | `balanco` | 0% = −5°, 50% = 5°, 100% = −5°. O fim é igual ao começo, então não há pulo. |
| `alternate` | `onda` | só `from` e `to`; o `alternate` faz a volta sozinho |

### 11.5 Duas animações no mesmo elemento

A propriedade `animation` aceita **várias animações, separadas por vírgula**:

```css
animation: balanco 2s linear 1s infinite, navegar 10s linear 1s infinite;
```

⚠️ **Erro que já apareceu (atividade 10):** escrever `animation` duas vezes:

```css
/* ❌ a segunda linha substitui a primeira: o barco não balança */
animation: balanco 2s linear 1s infinite;
animation: navegar 10s linear 1s infinite;

/* ✅ uma linha só, com vírgula */
animation: balanco 2s linear 1s infinite, navegar 10s linear 1s infinite;
```

🔗 É a regra da cascata (seção 2.3): a mesma propriedade escrita duas vezes → vale a última.

⚠️ **Duas animações não podem mexer na mesma propriedade.** Se o `balanco` usasse `transform: rotate(...)` e o `navegar` usasse `transform: translateX(...)`, uma substituiria a outra, e só um dos movimentos apareceria. A solução usada foi dar uma propriedade diferente para cada animação:

- `balanco` → `transform` (gira);
- `navegar` → `left` (anda).

Outra saída é colocar uma animação numa div "pai" e a outra numa div "filha".

### 11.6 Atravessar a tela em loop sem "pulo"

```css
@keyframes navegar {
    0%   { left: -100px; }   /* começa inteiro fora da tela, à esquerda */
    100% { left: 100%; }     /* termina inteiro fora da tela, à direita */
}
```

- `-100px` é a largura do barco: no início, ele está todo escondido à esquerda.
- `100%` é a largura da janela: no fim, o lado esquerdo do barco está na borda direita, ou seja, ele está todo escondido.
- Quando a animação recomeça, o barco "salta" de volta para a esquerda, mas ninguém vê, porque nos dois momentos ele está fora da tela.
- O `overflow: hidden` no `body` impede que apareça uma barra de rolagem quando ele passa da borda.

A nuvem usa a mesma ideia, mas começa em `-120px`, porque o desenho dela, com as cópias do `box-shadow`, tem 100px de largura (seção 10.2).

💡 **O atraso de 1s tem um efeito colateral:** durante esse segundo, a animação ainda não começou, então o barco aparece parado no lugar "normal" dele (encostado à esquerda); depois ele some e entra pela esquerda. Para evitar, tire o atraso ou acrescente `animation-fill-mode: backwards;`, que já mostra o 0% do roteiro durante a espera.

### 11.7 O que dá para animar

Dá para animar propriedades que têm **números ou cores**: `left`, `top`, `width`, `opacity` (transparência, de 0 a 1), `color`, `background-color`, `transform`, `background-position`...

Não dá para animar de forma suave o que é "liga/desliga" ou não tem número, como `display` e `font-family`.

💡 `transform` e `opacity` são as mais "leves" para o navegador: animações com elas ficam mais suaves.

### 11.8 🧩 Juntando tudo: a cena da atividade 10

```
┌────────────────────────────────────────────┐ ← body: céu (linear-gradient)
│  (nuvem) ──────────────→                   │ ← top: 60px; atravessa em 20s
│                                            │
│                 [barco] ──────→            │ ← bottom: 130px; z-index: 2
│/¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯\/¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯\│ ← ondas: ::before e ::after
│████████████████████████████████████████████│ ← .mar: bottom: 0;
│████████████████████████████████████████████│   height: 150px; z-index: 1
└────────────────────────────────────────────┘
```

| Elemento | Técnicas usadas | Seções |
|---|---|---|
| céu | `linear-gradient` no `body` + `height: 100vh` + `margin: 0` | 8.2, 2.5 |
| barco | `background-image` + `position: absolute` + `z-index` + duas animações (balanço com `transform`, travessia com `left`) | 8.1, 6, 11 |
| mar | `position: absolute; bottom: 0` + `z-index` | 6 |
| ondas | `::before` / `::after` + `border-radius` + animação com `alternate` | 9, 8.4, 11.4 |
| nuvem | `border-radius` + `box-shadow` (cópias) + animação com `left` | 8.4, 10.2, 11.6 |
| sem barra de rolagem | `overflow: hidden` no `body` | 4.2 |

---

## 12. Erros que já apareceram e como investigar

### 12.1 Os erros, um por um

**1. O layout virou várias colunas (atividade 9)**

- **Por quê:** o `</div>` do `#box1` fechava logo depois de "Pessoal". O resto do conteúdo virou filho direto do container flex, e todo filho direto de flex vira um item (uma coluna).
- **Solução:** fechar o `#box1` só depois de todo o conteúdo dele (seção 5.7).

**2. Espaço estranho no endereço (atividade 9)**

- **Por quê:** `<p class="dados">Rua da Paz, 250<p>`, com `<p>` no lugar de `</p>`. O navegador abriu um parágrafo novo, vazio, que tem margem própria.
- **Solução:** fechar com `</p>`.

**3. Tag sem fechar (atividade 9)**

- **Por quê:** a `<div class="container">` não tinha `</div>`. O navegador tenta "adivinhar" onde fechar; às vezes acerta, às vezes não.
- **Solução:** fechar toda tag aberta. Só algumas tags não têm fechamento, como `<hr>`, `<img>`, `<meta>` e `<input>`.

**4. A lateral perdeu a altura (atividade 9)**

- **Por quê:** os dois boxes estavam com `position: absolute`. O `absolute` tira o elemento do fluxo, então ele não conta para a altura do pai. O container ficou com altura 0, e o `height: 100%` do `#box1` virou 100% de 0.
- **Solução:** para duas colunas lado a lado, `display: flex` no container é mais simples e seguro (seção 5.6). Guarde o `absolute` para elementos pontuais, como um botão num canto.

**5. "A cor não mudou" (atividade 9)**

- **Por quê:** mudou, sim. `#3d4450` é um cinza-azulado quase preto.
- **Solução:** testar com uma cor chamativa (`red`) antes de escolher a definitiva (seção 3.1).

**6. Espaçamento diferente no email (atividade 9)**

- **Por quê:** o email estava numa `<div>`, e os outros dados em `<p>`. O `<p>` tem margem padrão do navegador; a `<div>`, não (seção 2.5).
- **Solução:** usar a mesma tag para itens do mesmo tipo.

**7. A animação não rodava (atividade 10)**

- **Por quê:** o `transform:` estava escrito direto dentro do `@keyframes`, sem dizer em que momento.
- **Solução:** colocar cada propriedade dentro de um momento (`0%`, `50%`, `from`, `to`...) (seção 11.3).

**8. O barco andava, mas não balançava (atividade 10)**

- **Por quê:** o `animation:` estava escrito duas vezes no `.barco`, e a segunda substituía a primeira.
- **Solução:** uma linha só, com as animações separadas por vírgula (seção 11.5).

**9. A imagem do barco cortada ou repetida (atividade 10)**

- **Por quê:** faltavam `background-size: contain` e `background-repeat: no-repeat`.
- **Solução:** ver seção 8.1.

**10. A nuvem não aparecia (atividade 10)**

- **Por quê:** nuvem branca num fundo branco. Ela estava lá, mas invisível.
- **Solução:** fazer o céu com `linear-gradient` (seção 8.2).

**11. O `float` era ignorado (atividade 7)**

- **Por quê:** o pai do elemento tinha `display: flex`.
- **Solução:** colocar o elemento dentro de uma `div` comum (seção 7.3).

### 12.2 "Só funciona se...": as dependências do CSS

Muitos "não funciona" acontecem porque uma propriedade depende de outra. Esta tabela junta todas que apareceram aqui:

| Isto... | Só funciona se... | Seção |
|---|---|---|
| `justify-content`, `align-items`, `gap` | o elemento tiver `display: flex` (ou `grid`) | 5.5 |
| `flex`, `align-self`, `order` | o **pai** tiver `display: flex` | 5.6 |
| `top`, `right`, `bottom`, `left` | o `position` não for `static` | 6.2 |
| `position: absolute` ficar dentro do pai | o pai tiver `position` (normalmente `relative`) | 6.3 |
| `z-index` | o `position` não for `static` (ou o elemento for filho de flex/grid) | 6.6 |
| `::before` e `::after` aparecerem | eles tiverem `content` | 9.2 |
| `height: 100%` | o pai tiver altura definida | 4.1 |
| `width` e `height` | o elemento não for `inline` (como `span` e `a`) | 5.2 |
| `margin: 0 auto` centralizar | o elemento for `block` e tiver `width` | 4.3 |
| `float` | o pai **não** for flex nem grid | 7.3 |
| a imagem de fundo aparecer numa div vazia | a div tiver `width` e `height` | 8.1 |
| um `@keyframes` rodar | algum elemento usar o nome dele em `animation` | 11.2 |
| o degradê do `body` cobrir a tela | o `body` tiver altura (`height: 100vh`) | 8.2 |

### 12.3 Checklist: quando algo não funciona

1. Salvei o arquivo e atualizei o navegador (**F5**, ou **Ctrl + F5** para forçar)?
2. O seletor está certo? `.` para classe, `#` para id, espaço ou sem espaço? O nome é **igual** ao do HTML (maiúsculas, acentos, hífens)?
3. Falta algum `;`, `:` ou `}`?
4. A propriedade depende de outra? (tabela 12.2)
5. Uma regra mais forte está ganhando? (especificidade, seção 2.4)
6. Escrevi a mesma propriedade duas vezes na mesma regra?
7. As tags estão abertas e fechadas no lugar certo?
8. A cor é parecida demais com o fundo? Teste com `red`.

### 12.4 Usando o DevTools do navegador (F12)

- Clique com o botão direito no elemento → **Inspecionar**.
- **Aba Styles (Estilos):** mostra todas as regras que atingem o elemento.
  - Declarações **riscadas** perderam a disputa: outra regra mais forte ganhou (seção 2.4).
  - Um ícone de alerta ao lado da declaração indica valor inválido (erro de digitação).
  - Dá para clicar e mudar os valores ao vivo para testar. Isso não salva no arquivo.
- **Aba Computed (Calculado):** mostra o desenho do box model (margin, border, padding e conteúdo) com os números de verdade.
- Passar o mouse sobre o HTML na aba **Elements** destaca a caixa correspondente na página.

💡 **No VS Code**, **Shift + Alt + F** formata o documento (arruma a indentação). Com o código indentado, uma tag fechada no lugar errado aparece na hora.

---

## 13. Teste seus conhecimentos

Tente responder antes de abrir a resposta.

<details>
<summary>1. Qual a diferença entre <code>#japao.bandeira</code> e <code>#japao .bandeira</code>?</summary>

Colado (`#japao.bandeira`) é **um mesmo elemento** com id `japao` e classe `bandeira`. Com espaço (`#japao .bandeira`) é um elemento com classe `bandeira` **dentro** do `#japao`. (Seção 2.2)

</details>

<details>
<summary>2. Quem ganha: <code>.menu.ativo</code> ou <code>#topo</code>?</summary>

`#topo`. A pontuação dele é (1,0,0), contra (0,2,0) do outro. Um id vence qualquer quantidade de classes. (Seção 2.4)

</details>

<details>
<summary>3. Por que o texto não centraliza? <code>.box { justify-content: center; align-items: center; }</code></summary>

Falta `display: flex;`. Sem ele, `justify-content` e `align-items` não têm efeito. (Seção 5.5)

</details>

<details>
<summary>4. Um elemento com <code>position: absolute; top: 0; left: 0;</code> foi parar no canto da página, e não no canto da div pai. O que falta?</summary>

`position: relative` no pai. Sem ele, o `absolute` não encontra um ancestral posicionado e usa a página como referência. (Seção 6.3)

</details>

<details>
<summary>5. Por que esta div não aparece? <code>.barco { background-image: url("barco.png"); }</code></summary>

Faltam `width` e `height`. A div está vazia, então tem altura 0, e a imagem de fundo não dá tamanho à caixa. (Seção 8.1)

</details>

<details>
<summary>6. O barco só anda e não balança. O CSS tem <code>animation: balanco 2s infinite;</code> e, na linha de baixo, <code>animation: navegar 10s linear infinite;</code>. Por quê?</summary>

A segunda linha substitui a primeira (mesma propriedade duas vezes → vale a última). O certo é juntar com vírgula: `animation: balanco 2s infinite, navegar 10s linear infinite;`. (Seção 11.5)

</details>

<details>
<summary>7. O que acontece se você apagar o <code>content: "";</code> do <code>.mar::before</code>?</summary>

A onda some. Sem `content`, o pseudo-elemento não é criado. (Seção 9.2)

</details>

<details>
<summary>8. Com <code>width: 200px; padding: 10px; border: 2px solid black;</code> e o <code>box-sizing</code> padrão, qual a largura na tela?</summary>

2 + 10 + 200 + 10 + 2 = **224px**. Com `box-sizing: border-box`, seriam 200px. (Seção 4.4)

</details>

<details>
<summary>9. Um container flex tem 1000px. O filho A tem <code>width: 300px</code>, e o filho B tem <code>flex: 1</code>. Qual a largura do B?</summary>

**700px**: o B fica com todo o espaço que sobra (1000 − 300). (Seção 5.6)

</details>

<details>
<summary>10. <code>border-radius: 50%</code> num elemento de 200px × 100px vira o quê?</summary>

Uma **oval**. Num quadrado, viraria um círculo. (Seção 8.4)

</details>

<details>
<summary>11. Um elemento com <code>position: static</code> e <code>z-index: 10</code> fica na frente dos outros?</summary>

Não. O `z-index` precisa de `position` diferente de `static` (a exceção são os filhos de flex e grid). (Seção 6.6)

</details>

<details>
<summary>12. Em <code>animation: nuvem 20s linear 3s infinite;</code>, o que é o <code>3s</code>?</summary>

O **atraso**. O primeiro tempo é a duração (20s), e o segundo é o atraso. (Seção 11.4)

</details>

---

## 14. Resumo para revisão

**Fundamentos e seletores**

- Regra = seletor + `{ propriedade: valor; }`. O CSS não mostra erro: o que ele não entende, ignora.
- `.classe` pode repetir; `#id` é único. Colado = o mesmo elemento; espaço = dentro; `>` = filho direto; `+` = irmão logo depois; `,` = grupo.
- Quem ganha uma disputa: maior especificidade (ids > classes e atributos > tags). Empate → a regra escrita por último. Inline ganha de todos.
- A mesma propriedade escrita duas vezes → vale a última.

**Texto e caixa**

- Propriedades de texto (`color`, `font-*`) são herdadas; as de caixa (`width`, `border`, `margin`, `background`) não. Uma regra direta no elemento vence a herança.
- Box model, de dentro para fora: conteúdo → `padding` → `border` → `margin`. Com `content-box` (o padrão), padding e borda somam no tamanho.
- O `%` é sempre "de alguma coisa": `width` → do pai; `translate` e `border-radius` → do próprio elemento.
- Uma div vazia e sem `height` tem altura 0.

**`display` e flex**

- `block` ocupa a linha; `inline` fica no texto e ignora tamanho; `inline-block` fica na linha e aceita tamanho; `none` some.
- `display: flex` no pai organiza os **filhos diretos**. `justify-content` = eixo principal; `align-items` = eixo cruzado. Com `column`, eles trocam.
- `flex: 1` no filho = ocupar o espaço que sobra.
- Todo filho direto de flex vira item: confira onde cada `</div>` fecha.

**`position`**

- `display`: o pai organiza os filhos. `position`: o elemento escolhe onde fica.
- `relative` não sai do fluxo e vira referência; `absolute` sai do fluxo e se posiciona pelo ancestral posicionado mais próximo.
- Um filho `absolute` não conta para a altura do pai.
- `top`, `left` e `z-index` precisam de `position` diferente de `static`. `z-index` maior = mais na frente.
- Centralizar: `top: 50%; left: 50%; transform: translate(-50%, -50%);`.

**`float`**

- `float` empurra para um lado, e o conteúdo seguinte contorna; `clear: both` faz começar embaixo. Não funciona em filho de flex.

**Fundos, formas e sombras**

- `background-image` precisa de uma caixa com tamanho. Use `background-size` e `background-repeat`.
- Gradientes vão em `background-image`. Duas cores com a mesma porcentagem = corte seco.
- `border-radius: 50%` → círculo (num quadrado) ou oval (num retângulo). `clip-path: polygon()` recorta qualquer forma.
- `box-shadow` sem desfoque e da mesma cor = uma cópia do elemento (a nuvem). Vírgula = várias sombras.
- `::before` e `::after` precisam de `content: ""`. Com `position: absolute`, viram desenhos dentro do pai.

**Animação**

- `@keyframes` = o roteiro (momentos em `%`); `animation` = o play (nome, duração, ritmo, atraso, repetições, direção).
- Primeiro tempo = duração; segundo tempo = atraso.
- Várias animações: uma linha só, separadas por vírgula. Duas animações não devem mexer na mesma propriedade.
- `alternate` = vai e volta; `linear` = velocidade constante; `infinite` = repete para sempre.
- Para atravessar a tela em loop: começar e terminar fora da tela, com `overflow: hidden` no pai.
