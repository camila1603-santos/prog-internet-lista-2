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

| Propriedade | O que faz |
|---|---|
| `display: flex` | liga o "modo organizador" |
| `justify-content` | controla a posição dos filhos na horizontal |
| `align-items` | controla a posição dos filhos na vertical |
| `flex-direction` | define se os filhos ficam em linha ou em coluna |
| `gap` | define o espaço entre os filhos |

> ⚠️ Essas propriedades só funcionam junto com `display: flex`. Sem ele, `justify-content` e `align-items` não fazem nada.

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

## 📌 Para guardar

- Na prática, **90% do tempo** você vai usar `block`, `inline`, `inline-block`, `none`, `flex` e `grid`. Os outros você consulta quando precisar.
- O `display` define o comportamento **por fora** (com os vizinhos) e **por dentro** (com os filhos).
- `justify-content` e `align-items` **dependem** de `display: flex` ou `grid`.
- **Tipo 1** (`width`, `height`, `background-color`...) muda o próprio elemento. **Tipo 2** (`justify-content`, `align-items`, `gap`...) vai no pai e organiza os filhos.
- Um elemento pode ser pai e filho ao mesmo tempo, então pode ter os dois tipos de propriedade.
