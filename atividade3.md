# Atividade 3

## Como podemos incluir CSS em documentos HTML?

Há três formas de incluir CSS em um documento HTML.

### 1. Inline

No atributo `style` do próprio elemento.

```html
<p style="color: blue;">Texto</p>
```

### 2. Interno

Dentro da tag `<style>`, no `<head>`.

```html
<head>
    <style>
        p { color: blue; }
    </style>
</head>
```

### 3. Externo

Em um arquivo `.css` separado, ligado pela tag `<link>` no `<head>`.

```html
<head>
    <link rel="stylesheet" href="estilo.css">
</head>
```

O externo é o mais recomendado, porque o mesmo arquivo pode ser usado em várias páginas.
