# Guia de Design — Luan Sinézio (marca pessoal)

> Sistema visual do perfil pessoal do Luan (@luansinezio), founder da Beza Media.
> Versão renderizada, com os dois temas: `marca/manual-de-marca-luan.html`.
> A marca da agência tem guia próprio em `marca/design-guide.md`. Os dois não se misturam.

---

## O que separa esse sistema do da Beza

Mesmo motor, cor diferente. IBM Plex Sans e IBM Plex Mono, peso Medium 500 em heading,
letter-spacing negativo, grão em todo fundo, eyebrow com dot: tudo isso continua igual.

O que muda:

1. **Cinza neutro de verdade.** Todo hex de superfície e de tipografia tem R=G=B exato, desvio 0.
   O cinza da Beza tem azul dentro (`#0A0A10` tem 6 pontos de desvio, `#8B8DA1` tem 22, `#606282` tem 34).
2. **Saturação existe só dentro do retângulo da foto.** Tipografia, ícone, borda, régua, fundo chapado,
   scrim e ruído ficam em cinza puro em qualquer peça.
3. **A ênfase vem do valor, não da cor.** Sem lilás pra destacar a palavra-chave, quem carrega a tese
   sobe pro topo da rampa e a linha de apoio desce. Essa inversão não é opcional: sem ela o destaque
   some no escuro e fica ilegível no claro.
4. **O lilás `#9A9BE5` só entra em peça de collab com a Beza**, na dose e nos lugares da seção "Collab".
5. **O claro é cinza papel, não quase-branco.** As artes dele já vivem em `#D2D2D2` e `#949494`.

---

## Cores

### Tema escuro (padrão)

```css
:root{
  --bg:#0E0E0E;          /* o chão da página e do post */
  --bg-soft:#1A1A1A;     /* seção alternada */
  --bg-card:#242424;     /* CHUMBO: card e bloco preenchido */
  --text:#E8E8E8;        /* corpo e título */
  --text-muted:#A8A8A8;  /* texto secundário */
  --text-dim:#8C8C8C;    /* metadata, legenda, crédito de foto */
  --border:#333333;      /* fio de 1px entre blocos */
  --border-strong:#757575;
  --accent:#F5F5F5;      /* ênfase, o teto do sistema */
  --cinza-claro:#D4D4D4; /* texto de apoio em corpo grande */
}
```

### Tema claro

```css
:root[data-tema="claro"]{
  --bg:#EAEAEA;          /* cinza papel */
  --bg-soft:#E0E0E0;
  --bg-card:#F5F5F5;     /* papel baritado, o teto do sistema */
  --text:#1C1C1C;
  --text-muted:#4F4F4F;
  --text-dim:#616161;
  --border:#D4D4D4;
  --border-strong:#7A7A7A;
  --accent:#242424;      /* CHUMBO vira a ênfase */
}
```

### Rampa neutra (mesma escala nos dois temas)

`#F5F5F5 · #E8E8E8 · #D4D4D4 · #A8A8A8 · #8C8C8C · #757575 · #4F4F4F · #333333 · #242424 · #1A1A1A · #0E0E0E`

Os dois nomes que o Luan usa falando: **chumbo** é `#242424`, **cinza claro** é `#D4D4D4`.

### Contraste conferido (WCAG, calculado)

| Par | Escuro | Claro |
|-----|--------|-------|
| text / bg | 15,76:1 | 14,17:1 |
| text / bg-card | 12,67:1 | 15,63:1 |
| text-muted / bg | 8,12:1 | 6,81:1 |
| text-dim / bg-card | 4,62:1 | 5,68:1 |
| accent / bg | 17,71:1 | 12,90:1 |

O par mais apertado do sistema é o terciário dentro de card, em 4,62:1, acima dos 4,5 exigidos em corpo.
Borda (`#333333` no escuro, `#D4D4D4` no claro) é fio de composição e nunca serve como único indicador de estado.

### Proibido

- Preto absoluto `#000000` e branco puro `#FFFFFF` em fundo e em tipografia. Os dois só existem dentro da foto.
- Cinza com temperatura: o lilás da Beza e o cinza azulado do Tailwind.
- Lilás `#9A9BE5` fora de peça de collab.
- Fundo híbrido misturando o `#0E0E0E` dele com o `#0A0A10` da Beza. A diferença de 1,09 de L* lê como erro de exportação.

---

## Foto

Foto sangra os quatro lados, sempre. Zero moldura, zero margem branca, zero canto arredondado.

Dentro do retângulo da foto o preto pode chegar a `#000000` e a alta pode estourar em `#FFFFFF`,
porque ali é fotografia e ela tem a própria régua. O chão da página nunca chega lá.

**Rota A, foto como campo** (o caso do "você está ficando confortável"): saturação 0,0 exato,
subexposta, motion blur, grão empurrado, vinheta com queda de 12 a 15 pontos de luminância da borda pro centro.
A foto vira fundo e o texto corre por cima.

**Rota B, foto como cena** (o caso do "perder o sono"): o quente entra em três lugares e só neles,
fonte de luz prática (abajur, poste, janela de fim de tarde, luz de tela), pele e madeira.
Céu, parede, piso, asfalto e roupa neutra ficam dessaturados na correção.
Matiz entre 20 e 45 graus, temperatura de cena entre 2700K e 3400K. Nada de verde, ciano ou magenta.

---

## Collab com a Beza

O lilás entra em três casos, e nada além deles:

1. A logo da Beza está no quadro.
2. A peça vende a agência e sai no perfil dele.
3. O conteúdo sai nos dois perfis com selo de collab.

Falar da Beza e assinar com a Beza são coisas diferentes. Post que conta case de cliente, número de folha
ou contratação errada continua 100% neutro: ali ele é o autor e a agência é o assunto.

Dose máxima: 2 elementos e teto de 5% da área do quadro, somados. O fundo nunca vira lilás
e a tipografia inteira nunca vira lilás.

---

## Tipografia

Igual à Beza: `'IBM Plex Sans'` em corpo e heading, `'IBM Plex Mono'` em eyebrow, label, ano e categoria.
Medium 500 é o peso padrão de heading, Regular 400 em corpo. Heading em sentence case, sempre.
Letter-spacing negativo em heading (`-0.02em`), positivo alto em mono (`0.18em` a `0.22em`).

A diferença: **a ênfase inline não pode vir de cor.** Vem de valor (a palavra-chave sobe pro `--accent`),
de tamanho (o salto de escala dentro da mesma frase) ou de caixa. Negrito inline continua fora.

---

*Guia iniciado em 27/ago/2026. As seções de textura, luz, espaçamento, display sobre foto, aplicações e
o que nunca fazer estão detalhadas e renderizadas em `marca/manual-de-marca-luan.html`.*
